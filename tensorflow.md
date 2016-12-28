tensorflow: difference between multi GPUs and distributed tensorflow



It depends a little on the perspective from which you look at it. In any multi-* setup, either multi-GPU or multi-machine, you need to decide how to split up your computation across the parallel resources. In a single-node, multi-GPU setup, there are two very reasonable choices:

(1) **Intra-model parallelism**. If a model has long, independent computation paths, then you can split the model across multiple GPUs and have each compute a part of it. This requires careful understanding of the model and the computational dependencies.

(2) **Replicated training**. Start up multiple copies of the model, train them, and then synchronize their learning (the gradients applied to their weights & biases).

Our [released Inception model](https://github.com/tensorflow/models/tree/master/inception) has some good diagrams in the readme that show how both multi-GPU and distributed training work.

But to tl;dr that source: In a multi-GPU setup, it's often best to synchronously update the model by storing the weights on the CPU (well, in its attached DRAM). But in a multi-machine setup, we often use a separate "parameter server" that stores and propagates the weight updates. To scale that to a lot of replicas, you can shard the parameters across multiple parameter servers.

With multiple GPUs and parameter servers, you'll find yourself being more careful about device placement using constructs such as with `tf.device('/gpu:1')`, or placing weights on the parameter servers using [tf.train.replica_device_setter](https://www.tensorflow.org/versions/r0.9/api_docs/python/train.html#replica_device_setter) to assign it on `/job:ps` or `/job:worker`.

In general, training on a bunch of GPUs in a single machine is much more efficient -- it takes more than 16 distributed GPUs to equal the performance of 8 GPUs in a single machine -- but distributed training lets you scale to even larger numbers, and harness more CPU.


sudo init 3
./cuda_8.0.44_linux.run 
cd pip-9.0.1/
python setup.py install
pip install pbr-1.10.0-py2.py3-none-any.whl 
pip install mock-2.0.0-py2.py3-none-any.whl 
pip install protobuf-3.1.0-py2.py3-none-any.whl 
pip install tensorflow_gpu-0.12.0rc0-cp27-none-linux_x86_64.whl 

export CUDA_HOME=/usr/local/cuda-8.0
export LD_LIBRARY_PATH="/usr/local/cuda-8.0/lib64:/usr/local/cuda-8.0/extras/CUPTI/lib64:$LD_LIBRARY_PATH"

export CUDA_SO="$(\ls /usr/lib64/libcuda* | xargs -I{} echo '-v {}:{}') $(\ls /usr/lib64/libnvidia* | xargs -I{} echo '-v {}:{}')"
export DEVICES=$(\ls /dev/nvidia* | xargs -I{} echo '--device {}:{}')


sudo docker run --privileged --name t2 -d -P ${CUDA_SO} ${DEVICES} -v /usr/local/cuda-8.0:/usr/local/cuda -it tensorflow/tensorflow:latest-devel bash

export LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH

要求大写：
export HTTP_PROXY=http://10.214.129.29:8118/     
export HTTPS_PROXY=http://10.214.129.29:8118/

build tensorflow:
./configure



# On ps0.example.com:
$ python trainer.py \
     --ps_hosts=10.214.129.14:2222,10.214.129.15:2222,10.214.129.29:2222 \
     --worker_hosts=10.214.129.14:2223,10.214.129.15:2223,10.214.129.29:2223 \
     --job_name=ps --task_index=0
# On ps1.example.com:
$ python trainer.py \
     --ps_hosts=10.214.129.14:2222,10.214.129.15:2222,10.214.129.29:2222 \
     --worker_hosts=10.214.129.14:2223,10.214.129.15:2223,10.214.129.29:2223 \
     --job_name=ps --task_index=1
$ python trainer.py \
     --ps_hosts=10.214.129.14:2222,10.214.129.15:2222,10.214.129.29:2222 \
     --worker_hosts=10.214.129.14:2223,10.214.129.15:2223,10.214.129.29:2223 \
     --job_name=ps --task_index=2
# On worker0.example.com:
$ python trainer.py \
     --ps_hosts=10.214.129.14:2222,10.214.129.15:2222,10.214.129.29:2222 \
     --worker_hosts10.214.129.14:2223,10.214.129.15:2223,10.214.129.29:2223 \
     --job_name=worker --task_index=0
# On worker1.example.com:
$ python trainer.py \
     --ps_hosts=10.214.129.14:2222,10.214.129.15:2222,10.214.129.29:2222 \
     --worker_hosts=10.214.129.14:2223,10.214.129.15:2223,10.214.129.29:2223 \
     --job_name=worker --task_index=1
$ python trainer.py \
     --ps_hosts=10.214.129.14:2222,10.214.129.15:2222,10.214.129.29:2222 \
     --worker_hosts=10.214.129.14:2223,10.214.129.15:2223,10.214.129.29:2223 \
     --job_name=worker --task_index=2
