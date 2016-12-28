    1  df -lh
    2  init 3
    3  cd /home/amax/Desktop/
    4  ll
    5  chmod +x caffeinstall.txt centos\ cuda7.5ç¯å¢ƒå˜é‡.txt cuda_7.5.18_linux.run NVIDIA-Linux-x86_64-352.99.run teamviewer_11.0.57095.i686.rpm 
    6  ll
    7  ./cuda_7.5.18_linux.run 
    8  ll
    9  ./NVIDIA-Linux-x86_64-352.99.run --no-opengl-files
   10  init 6
   11  nvcc -V
   12  teamviewer 
   13  nvidia-smi 
   14  vim /etc/profile.d/set.sh
   15  source /etc/profile.d/set.sh
   16  nvcc -V
   17  vim /etc/sysconfig/network-scripts/
   18  cd /etc/sysconfig/network-scripts/
   19  ls
   20  vim ifcfg-enp129s0f0
   21  init 0
   22  vim /etc/sysconfig/network-scripts/
   23  cd /etc/sysconfig/network-scripts/
   24  ls
   25  vim ifcfg-enp129s0f0
   26  init 0
   27  ll
   28  cp -ra * /opt/
   29  cd /opt/
   30  ll
   31  chmod x cmake-3.0.2.tar.gz cudnn-7.5-linux-x64-v5.1.solitairetheme8 glog-0.3.3.tar.gz hdf5-1.8.17.tar leveldb-master.zip pip-8.1.2.tar.gz protobuf-2.5.0.tar.gz tensorflow-0.11.0rc0-cp27-none-linux_x86_64.whl 
   32  chmod +x cmake-3.0.2.tar.gz cudnn-7.5-linux-x64-v5.1.solitairetheme8 glog-0.3.3.tar.gz hdf5-1.8.17.tar leveldb-master.zip pip-8.1.2.tar.gz protobuf-2.5.0.tar.gz tensorflow-0.11.0rc0-cp27-none-linux_x86_64.whl 
   33  ll
   34  tar zxvf cudnn-7.5-linux-x64-v5.1.solitairetheme8 
   35  cp -ra cuda/* /usr/local/cuda
   36  nvcc -V
   37  source /etc/profile.d/set.sh 
   38  ll
   39  yum install protobuf-devel leveldb-devel snappy-devel opencv-devel boost-devel hdf5-devel
   40  tar zxvf cmake-3.0.2.tar.gz 
   41  cd cmake-3.0.2/
   42  ./bootstrap 
   43  ./configure --prefix=/mnt/cmake
   44  make && make install
   45  vim /etc/profile.d/set.sh 
   46  source /etc/profile.d/set.sh 
   47  cd ..
   48  tar zxvf glog-0.3.3.tar.gz 
   49  cd glog-0.3.3/
   50  ./configure 
   51  make && make install
   52  cd ..
   53  wget https://github.com/schuhschuh/gflags/archive/master.zip 
   54  chmos +x master.zip 
   55  unzip master.zip 
   56  cd gflags-master/
   57  mkdir build && cd build 
   58  export CXXFLAGS="-fPIC" && cmake .. && make VERBOSE=1
   59  make && make install
   60  git clone https://github.com/LMDB/lmdb 
   61  cd lmdb/libraries/liblmdb/
   62  make && make install
   63  cd ..
   64  cd /opt/
   65  ll
   66  cd gflags-master/build/
   67  ll
   68  rm -rf lmdb
   69  ll
   70  cd ..
   71  cd /opt/
   72  git clone https://github.com/LMDB/lmdb
   73  cd lmdb/libraries/liblmdb/
   74  make && make install
   75  cd /opt/
   76  tar xf hdf5-1.8.17.tar 
   77  cd hdf5-1.8.17/
   78  ./configure --prefix=/opt/hdf5
   79  make && make install
   80  cd ..
   81  unzip leveldb-master.zip 
   82  cd leveldb-master/
   83  make
   84  cp -r include/leveldb /usr/local/include
   85  cd out-shared/
   86  cp lib* /usr/local/lib
   87  cd ../..
   88  tar zxvf protobuf-2.5.0.tar.gz 
   89  cd protobuf-2.5.0/
   90  ./configure --prefix=/opt/protobuf
   91  make && make install
   92  cd /opt/
   93  yum install atlas-devel
   94  cd /usr/lib64/atlas/
   95  ll
   96  ln -s libsatlas.so.3.10 libatlas.so 
   97  ln -s libsatlas.so.3.10 libcblas.so
   98  cd /o
   99  cd /opt/
  100  vim /etc/profile.d/set.sh 
  101  source /etc/profile.d/set.sh 
  102  nvcc -V
  103  protoc --version
  104  cd caffe/
  105  make all -j 8
  106  cd /opt/
  107  ll
  108  tar zxf pip-8.1.2.tar.gz 
  109  cd pip-8.1.2/
  110  python setup.py install
  111  pip --version
  112  ll
  113  cd /opt/
  114  piplist
  115  pip -list
  116  pip list
  117  ll
  118  pip install tensorflow-0.11.0rc0-cp27-none-linux_x86_64.whl 
  119  pip ilst
  120  pip list
  121  python
  122  df =h
  123  df -h
  124  init 0
  125  df -h
  126  vim /etc/fstab 
  127  gedit /etc/fstab 
  128  teamviewer 
  129  blkid
  130  mkfs.ext4 /dev/sda1
  131  mkfs.ext4 /dev/sdb1
  132  mkfs.ext4 /dev/sde1
  133  mkfs.ext4 /dev/sdf1
  134  mkdir /data1
  135  mkdir /data2
  136  mkdir /data3
  137  mkdir /data4
  138  blkid
  139  mount -a
  140  df -lh
  141  init 6
  142  cd /opt/
  143  ll
  144  rm -f master.zip 
  145  ll
  146  rm -f cmake-3.0.2.tar.gz 
  147  rm -f glog-0.3.3.tar.gz hdf5-1.8.17.tar leveldb-master.zip pip-8.1.2.tar.gz protobuf-2.5.0.tar.gz 
  148  ll
  149  rm -f cudnn-7.5-linux-x64-v5.1.solitairetheme8 
  150  ll
  151  init 0
  152  cd /opt/
  153  ll
  154  rm -f master.zip 
  155  ll
  156  rm -f cmake-3.0.2.tar.gz 
  157  rm -f glog-0.3.3.tar.gz hdf5-1.8.17.tar leveldb-master.zip pip-8.1.2.tar.gz protobuf-2.5.0.tar.gz 
  158  ll
  159  rm -f cudnn-7.5-linux-x64-v5.1.solitairetheme8 
  160  ll
  161  init 0
  162  cd /opt/
  163  ll
  164  rm -f master.zip 
  165  ll
  166  rm -f cmake-3.0.2.tar.gz 
  167  rm -f glog-0.3.3.tar.gz hdf5-1.8.17.tar leveldb-master.zip pip-8.1.2.tar.gz protobuf-2.5.0.tar.gz 
  168  ll
  169  rm -f cudnn-7.5-linux-x64-v5.1.solitairetheme8 
  170  ll
  171  init 0
  172  vim /etc/hostname 
  173  init 6
  174  vim /etc/hostname 
  175  su amax
  176  route 
  177  ping 10.209.100.24
  178  reboot
  179  exit
  180  passwd
  181  ip a
  182  useradd zhangyingfeng1
  183  passwd zhangyingfeng1
  184  exit
  185  vi /etc/ssh/sshd_config
  186  systemctl status sshd.service
  187  systemctl restart sshd.service
  188  systemctl status sshd.service
  189  ssh-keygen -t rsa 
  190  cd .ssh/
  191  ll
  192  scp root@10.214.128.50:/root/.ssh/authorized_keys .
  193  id lidachao1
  194  id zhangyingfeng1
  195  userdel zhangyingfeng1
  196  id zhangyingfeng1
  197  exit
  198  cd /home
  199  ll
  200  cd zhangyingfeng1/
  201  ll
  202  cd perl5/
  203  ll
  204  cd /home
  205  ll
  206  rm -rf zhangyingfeng1/
  207  ll
  208  id lidachao1
  209  su lidachao1
  210  exit
  211  useradd deepinsight
  212  passwd deepinsight
  213  exit
  214  vi /etc/sudoers
  215  exit
  216  cd /etc/yum
  217  vim /etc/yum.conf 
  218  pwd
  219  ls
  220  cd /etc/
  221  ls
  222  cd yum.repos.d/
  223  pwd
  224  ls
  225  yum date
  226  yum update
  227  ifcofnig
  228  ifconfig
  229  ps -elf | grep ssh
  230  yum upate
  231  yum update
  232  cd ..
  233  pwd
  234  wd
  235  pwd
  236  ls
  237  vim yum.conf 
  238  yum install docker
  239  vim /etc/yum.conf 
  240  ssh -C -f -N -g -D 8888 wangliang79@10.209.22.147
  241  pwd

sudo grub2-mkconfig -o /boot/grub2/grub.cfg

Cuda Error: CUDA driver version is insufficient for CUDA runtime version