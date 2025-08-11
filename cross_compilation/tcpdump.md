1.编译libpcap

编译命令：
``./configure --host=aarch64-buildroot-linux-gnu --with-pcap=linux--prefix=`pwd`/_install --disable-shared``

部分编译器不带libnl相关的库，需要自行编译libnl库，编译命令：
``./configure --host=aarch64-buildroot-linux-gnu --prefix=`pwd`/_install --disable-shared``

相应libpcap编译命令修改为：
``./configure --host=aarch64-buildroot-linux-gnu --with-pcap=linux--prefix=`pwd`/_install --disable-shared CFLAGS="-I/home/smb/third_src/libnl-3.2.25/_install/include/libnl3" LDFLAGS="-L/home/smb/third_src/libnl-3.2.25/_install/lib"``

2.编译tcpdump
将libpcap源码和tcpdump源码置于同一目录下
编译命令：
``./configure --host=aarch64-buildroot-linux-gnu --prefix=`pwd`/_install``