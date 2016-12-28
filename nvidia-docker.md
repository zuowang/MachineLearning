	login to 10.214.129.29
	deepinsight/deepinsight！123

	sudo su

	sudo yum update


	sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
	[dockerrepo]
	name=Docker Repository
	baseurl=https://yum.dockerproject.org/repo/main/centos/7/
	enabled=1
	gpgcheck=1
	gpgkey=https://yum.dockerproject.org/gpg
	EOF
	
	sudo yum install docker-engine
	
	sudo systemctl enable docker.service
	
	sudo systemctl start docker
	
	rpm -i nvidia-docker-1.0.0.rc.3-1.x86_64.rpm
	
	wget https://developer.nvidia.com/compute/cuda/8.0/prod/local_installers/cuda-repo-rhel7-8-0-local-8.0.44-1.x86_64-rpm


    `sudo rpm -i cuda-repo-rhel7-8-0-local-8.0.44-1.x86_64.rpm`
    `sudo yum clean all`
    `sudo yum install cuda`



## 安装cuda
完全多用户模式，标准的运行级
exit X before installation

	init 3 

chmod +x caffeinstall.txt cuda_7.5.18_linux.run NVIDIA-Linux-x86_64-352.99.run teamviewer_11.0.57095.i686.rpm

init 6:重启

./NVIDIA-Linux-x86_64-352.99.run --no-opengl-files