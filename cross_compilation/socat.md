使用socat命令实现将设备串口数据通过TCP传输到电脑端

交叉编译
```bash
./configure --prefix=/home/smb/third_src/socat-1.8.0.0/_install CC=arm-linux-gnueabihf-gcc --host=arm-linux-gnueabihf
make -j4
make install
```
命令执行
```bash
(弃用)socat -d -d /dev/ttyS3,b115200,raw,nonblock,ignoreeof,cr,echo=0 TCP4-LISTEN:10000,reuseaddr &
# /dev/ttyS3,b115200 设备串口节点,串口波特率115200
# -d -d socat详细打印，正式使用可忽略

#使用上一条命令，客户端断开链接后socat进程也会结束，在reuseaddr后加入fork可以防止socat退出
(弃用)socat /dev/ttyS3,b115200,raw,nonblock,ignoreeof,cr,echo=0 TCP4-LISTEN:10000,reuseaddr,fork &
#参考资料：https://serverfault.com/questions/655715/always-keep-socat-alive

#添加tcp缓冲区，限制每次写串口数据大小
(弃用)socat -b 1024 /dev/ttyS3,b115200,raw,nonblock,ignoreeof,cr,echo=0 TCP4-LISTEN:10000,reuseaddr,fork,so-rcvbuf=524288,so-sndbuf=524288 
#-b 1024 每次向串口输入最大数据1024

#ttyS3串口设置放在tcp-LISTEN之前，只有在执行命令的时候会去打开串口，发送完之后自动关闭，导致客户端第二次连接无法正常通信
#将ttyS3串口设置放在tcp-LISTEN之后，每次客户端连接重新打开串口，保证设备和串口正常通信
(弃用)socat -d -d -v -x -b 1024 TCP4-LISTEN:10000,reuseaddr,fork,so-rcvbuf=524288,so-sndbuf=524288 /dev/ttyS3,b115200,raw,nonblock,ignoreeof,cr,echo=0 &
#-v -x 16进制打印详细数据收发内容,正式使用可忽略

#socat使用中'cr'选项会将0x0a转换为0x0d
socat -b 1024 TCP4-LISTEN:10000,reuseaddr,fork,so-rcvbuf=524288,so-sndbuf=524288 /dev/ttyS3,b115200,raw,nonblock,ignoreeof,echo=0 &
#调试命令
#strace -o strace.log -f -xx -s 256 socat -d -d -v -x -b 1024 TCP4-LISTEN:10000,reuseaddr,fork,so-rcvbuf=524288,so-sndbuf=524288 /dev/ttyS3,b115200,raw,nonblock,ignoreeof,cr,echo=0
#strace用于跟踪系统调用，可以查看哪些调用可能导致失败
#-xx 以16进制显示数据内容
#-s 256 指定显示数据的最大长度，系统默认显示系统调用的前32个字节
```
[测试程序](https://github.com/Rambo-yb/serial-ts)

创建TCP服务器，打开节点/dev/ttyS3，使用select对套接字进行监控读取内容