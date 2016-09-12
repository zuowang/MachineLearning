对于读频繁的场景已经有RCU（read-copy update）：就是读不加锁，写操作做拷贝一份，往拷贝写，写完后，把拷贝的指针和原数据的指针做CAS（compare and swap）。这样可以保证非常高效的读

对于频繁写操作，OpLog: a library for scaling update-heavy data structures 提出OpLog：就是维护per-core log，写操作不加锁，读的时候加锁并合并这些log。

频繁写操作的难点：锁引入竞争使得写操作很慢，原子锁导致昂贵的cache line交换

oplog的三大优点：
1. 保证update scale，因为写只是简单的append to local per-core log，没有锁和cache line交换
2. logging和延迟执行，可以很方便地应用现有的操作
3. per-core log带来其他优化，比如放弃某些操作

poseidon的BgWorkerGroup创建一组SSPBgWorker线程类，它们继承AbstractBgWorker类，后者又继承Thread类。调用SSPBgWorker的Start方法会启动线程执行AbstractBgWorker中的operator()方法。


HandleAppendOpLogMsg方法中，根据table_id获得ClientTable，ClientTable有OpLog，代码中定义了DenseOpLog，SparseOpLog，AppendOnlyOpLog，AppendOnlyOpLogPartition，看下来只有AppendOnlyOpLogPartition在使用，所以ClientTable中的OpLog是AppendOnlyOpLogPartition。

	AbstractAppendOnlyBuffer *buff
      = table_iter->second->get_oplog().GetAppendOnlyBuffer(my_comm_channel_idx_);


AppendOnlyOpLogPartition有存放AbstractAppendOnlyBuffer的队列，MPMCQueue是个多生产者多消费者的队列。

	MPMCQueue<AbstractAppendOnlyBuffer*> shared_queue_;

在HandleAppendOpLogAndNotApply中把AbstractAppendOnlyBuffer增加到AppendOnlyRowOpLogBuffer

AppendOnlyRowOpLogBuffer记录在AbstractBgWorker的append_only_row_oplog_buffer_map_中，用于查重

AppendOnlyRowOpLogBuffer中的row_oplog_map_记录AbstractRowOpLog，它的BatchInc_负责添加AbstractRowOpLog，BatchInc_可为AppendOnlyRowOpLogBuffer::BatchIncGeneral或AppendOnlyRowOpLogBuffer::BatchIncDense。

	BatchIncGeneral(
	    AbstractRowOpLog *row_oplog,
	    const int32_t *col_ids, const void *updates, int32_t num_updates)
	
	BatchIncDense(
	    AbstractRowOpLog *row_oplog,
	    const int32_t *col_ids, const void *updates, int32_t num_updates)

BatchIncGeneral和BatchIncDense的区分是，BatchIncGeneral根据col_ids指定更新某些列，BatchIncDense忽略col_ids参数，更新所有列。它们更新列是调用AbstractRow的AddUpdates方法


AbstractRow类的AddUpdates可以看作静态类，所做的工作很简单，就是累加

	virtual void AddUpdates(int32_t column_id, void *update1,
	                const void *update2) const {
	  *(reinterpret_cast<V*>(update1)) += *(reinterpret_cast<const V*>(update2));
	}

调用AppendOnlyOpLogPartition::Inc和AppendOnlyOpLogPartition::BatchInc会调用append_only_buff_->Inc或->BatchInc,然后调用AppendOnlyOpLogPartition::FlushOpLog()，把append_only_buff_加入shared_queue_。

append_only_buff_只属于某线程，写不需加锁。append_only_buff_可选IncAppendOnlyBuffer，BatchIncAppendOnlyBuffer， DenseAppendOnlyBuffer。配置文件中append_only_oplog_type默认值为“Inc”，所以append_only_buff_默认为IncAppendOnlyBuffer。

IncAppendOnlyBuffer中Inc函数就是写入|row_id|col_id|update|到buff，这三部分都是定长的。BatchInc就相当于调用多次Inc

BatchIncAppendOnlyBuffer中BatchInc函数就是写入|row_id|num_updates|col_id|...|col_id|update|...|update|到buf，Inc函数则写入|row_id|1|col_id|update|到buf

调用流程：
Table::BatchInc->ClientTable(extend AbstractClientTable)::BatchInc->SSPPushAppendOnlyConsistencyController(extend AbstractConsistencyController)::BatchInc->AppendOnlyOpLogPartition(extend AbstractOpLog)::BatchInc + BgWorkers::SignalHandleAppendOnlyBuffer


继续调用流程：
AppendOnlyOpLogPartition(extend AbstractOpLog)::BatchInc->AbstractAppendOnlyBuffer::BatchInc->IncAppendOnlyBuffer::BatchInc

AbstractAppendOnlyBuffer::BatchInc如果写满buf了，会返回1，否则返回0
如果返回1，AppendOnlyOpLogPartition::BatchInc会调用，BgWorkers::SignalHandleAppendOnlyBuffer，后者调用BgWorkerGroup::SignalHandleAppendOnlyBuffer，后者调用AbstractBgWorker::SignalHandleAppendOnlyBuffer，后者发送BgHandleAppendOpLogMsg到bg_worker线程，线程调用AbstractBgWorker::HandleAppendOpLogMsg处理消息，apply oplog
