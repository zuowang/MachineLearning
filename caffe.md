### caffe编译

pull dokcer image

	docker pull tleyden5iwx/caffe-cpu-master
git clone caffe代码

	git clone https://github.com/01org/caffe.git
	git checkout -b multi_node 01org/multi_node

安装zeromq

	tar zxvf zeromq-4.0.4.tar.gz  
	cd zeromq-4.0.4  
	./configure --prefix=/usr/local/zeromq  
	make && make install  

设置环境变量

	cp /usr/local/zeromq/lib/pkgconfig/libzmq.pc /usr/lib/pkgconfig/
	export LD_LIBRARY_PATH=/usr/local/zeromq/lib
	export CPLUS_INCLUDE_PATH=/usr/local/zeromq/include

修改Makefile.config

	# BLAS choice:
	# atlas for ATLAS (default)
	# mkl for MKL
	# open for OpenBlas
	BLAS := atlas

编译caffe

	make all -j24

使用cmake编译caffe

	mkdir build
	cd build
	cmake ..
	make all
	make install