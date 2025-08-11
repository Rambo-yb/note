在源码目录下创建build目录，进入使用cmake进行编译。demo程序代码/(pocketsphinx目录)/porgrams/pocketsphinx\_main.c，音频文件形式传入数据识别示例代码/(pocketsphinx目录)/examples/simple.c，音频流形式传入数据识别示例代码/(pocketsphinx目录)/examples/live.c。

1.相关文档
网上的使用：[https://www.cnblogs.com/us-wjz/articles/11480260.html](https://www.cnblogs.com/us-wjz/articles/11480260.html)

pocketsphinx官网：[https://cmusphinx.github.io/](https://cmusphinx.github.io/)

语言模型库工具：[http://www.speech.cs.cmu.edu/tools/lmtool.html](http://www.speech.cs.cmu.edu/tools/lmtool.html)

2.编译命令
```shell
cd /(pocketsphinx目录)/;mkdir build;cd build

cmake -DCMAKE\_INSTALL\_PREFIX=/home/samba\_share/WorkSpace/pocketsphinx/\_install -DCMAKE\_C\_COMPILER=arm-rockchip830-linux-uclibcgnueabihf-gcc -DCMAKE\_CXX\_COMPILER=arm-rockchip830-linux-uclibcgnueabihf-g++ ../

make ; make install
```