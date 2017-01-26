faster-rcnn源码跟踪

faster-rcnn最困惑的地方是roidb怎么初始化的，我花了一天的时间跟踪，现在把roidb的初始化过程描述一下：

首先，给一段callstack

	  /home/deepinsight/py-faster-rcnn/tools/train_net.py(105)<module>()
	-> imdb, roidb = combined_roidb(args.imdb_name)
	  /home/deepinsight/py-faster-rcnn/tools/train_net.py(70)combined_roidb()
	-> roidbs = [get_roidb(s) for s in imdb_names.split('+')]
	  /home/deepinsight/py-faster-rcnn/tools/train_net.py(66)get_roidb()
	-> roidb = get_training_roidb(imdb)
	  /home/deepinsight/py-faster-rcnn/lib/fast_rcnn/train.py(118)get_training_roidb()
	-> imdb.append_flipped_images()
	> /home/deepinsight/py-faster-rcnn/lib/datasets/imdb.py(107)append_flipped_images()
	-> boxes = self.roidb[i]['boxes'].copy()
	  /home/deepinsight/py-faster-rcnn/lib/datasets/imdb.py(68)roidb()
	-> self._roidb = self.roidb_handler()
	  /home/deepinsight/py-faster-rcnn/lib/datasets/pascal_voc.py(93)gt_roidb()
	-> def gt_roidb(self):

代码中并没有地方刻意初始化roidb，是在第一次调用imdb.roidb的时候，初始化了roidb

callstack中可以看到，append_flipped_images是把翻转图像的roidb，append上去，所以最终的到roidb是num_images*2

在append_flipped_images的调用中，```self.roidb[i]['boxes'].copy()```第一次访问roidb

它会调用imdb.py中的roidb()，得到一个二维数组，再通过下标访问数组元素

roidb()中使用```self.roidb_handler()```初始化roidb，注意：roidb_handler()中```return self._roidb_handler```不是返回函数指针（像我这样，从C++转来做深度学习的人，估计会犯迷糊），它实际上是调用函数，返回函数结果

callstack中可以看到，它调用了```pascal_voc.py```中的gt_roidb()

get_roidb()会从cache文件中读取，如果没有cache文件，就读取annotation文件加载，这才是真正初始化roidb的地方

