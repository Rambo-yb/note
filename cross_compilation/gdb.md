```bash
export PATH="/home/smb/RV1126_RV1109_LINUX_SDK_V2.2.5.1_20230530/buildroot/output/rockchip_rv1126_rv1109/host/bin/:$PATH"

./configure --prefix=/home/smb/third_src/gmp-6.3.0/_install --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc --enable-shared=no

./configure --prefix=/home/smb/third_src/mpfr-4.2.1/_install --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc --with-gmp=/home/smb/third_src/gmp-6.3.0/_install --enable-shared=no

./configure --prefix=/home/smb/third_src/gdb-14.2/_install --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc CXX=arm-linux-gnueabihf-g++ --with-gmp=/home/smb/third_src/gmp-6.3.0/_install --with-mpfr=/home/smb/third_src/mpfr-4.2.1/_install --enable-shared=no
```


