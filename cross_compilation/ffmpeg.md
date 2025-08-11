脚本：
```shell
#!/bin/bash

BASE_PATH=`pwd`
TEMP_PATH=${BASE_PATH}/temp_install
OUTPUT_PATH=${BASE_PATH}/install

export PATH="/home/samba_share/rv1126_rv1109/prebuilts/gcc/linux-x86/arm/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin:$PATH"

tar -vxf libx264-git.tar.xz
cd ./libx264-git
#未添加--disable-opencl时，ffmpeg始终无法正常找到库和pkgconfig
./configure --prefix=${TEMP_PATH} --cross-prefix=arm-linux-gnueabihf- --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc --disable-asm --disable-opencl --enable-static
make
make install

cd ${BASE_PATH}
tar -zvxf x265_3.2.tar.gz
cd ./x265_3.2/build
cmake -DCMAKE_SYSTEM_NAME=linux -DCMAKE_SYSTEM_PROCESSOR=arm -DCMAKE_C_COMPILER=arm-linux-gnueabihf-gcc -DCMAKE_CXX_COMPILER=arm-linux-gnueabihf-g++ -DCMAKE_C_FLAGS="-ldl -lpthread" -DCMAKE_CXX_FLAGS="-ldl -lpthread" -DCMAKE_INSTALL_PREFIX=${TEMP_PATH} -DENABLE_SHARED=OFF ../source && make
make install

#pkg-config --list-all
export PKG_CONFIG_PATH="${TEMP_PATH}/lib/pkgconfig:$PKG_CONFIG_PATH"

cd ${BASE_PATH}
tar -vxf ffmpeg-4.1.tar.xz
cd ./ffmpeg-4.1
./configure --cross-prefix=arm-linux-gnueabihf- --enable-cross-compile --target-os=linux --cc=arm-linux-gnueabihf-gcc --arch=arm --prefix=${OUTPUT_PATH} --enable-static --enable-gpl --enable-nonfree --enable-swscale --enable-ffmpeg --disable-armv5te --disable-yasm --enable-libx264 --enable-libx265 --extra-cflags=-l${TEMP_PATH}include --extra-ldflags=-L${TEMP_PATH}lib --extra-libs="-lpthread" --pkg-config="pkg-config --static"
make
make install

cd ${BASE_PATH}
rm ./ffmpeg-4.1 -r
rm ./libx264-git -r
rm ./x265_3.2 -r
rm ${TEMP_PATH} -r
cp ${OUTPUT_PATH}/bin/ffmpeg ../../bin
mkdir ${BASE_PATH}/../../lib/ffmpeg
cp ${OUTPUT_PATH}/include ../../lib/ffmpeg/ -r
cp ${OUTPUT_PATH}/lib ../../lib/ffmpeg/ -r
rm ${OUTPUT_PATH} -r
```
命令：
```shell
ffmpeg -i testVideo.mp4 -c h264 testVideo.h264

ffmpeg -i testVideo.h264 -vf scale=1920\*1080 testVideoOut.h264

ffmpeg -i testVideo.mp4 -r 60 testVideoOut.mp4

ffmpeg -i testVideo.mp4 -r 60 -vf scale=1920\*1080 -c h264 testVideoOut.h264

# -r 帧率
# -vf scale=w\*h 分辨率
```

源码：
[https://www.ffmpeg.org/releases/](https://www.ffmpeg.org/releases/)

[https://johnvansickle.com/ffmpeg/release-source/](https://johnvansickle.com/ffmpeg/release-source/)

[https://bitbucket.org/multicoreware/x265\_git/downloads/](https://bitbucket.org/multicoreware/x265_git/downloads/)

资料：

http://t.zoukankan.com/EasyNVR-p-11505865.html

https://blog.csdn.net/qq\_41873311/article/details/125817314