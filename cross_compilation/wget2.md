```bash
./config no-asm no-shared --prefix=/home/samba_share/yuanbo/wget/_install --cross-compile-prefix=arm-rockchip830-linux-uclibcgnueabihf-

./configure --prefix=/home/samba_share/yuanbo/wget/_install --host=arm-rockchip830-linux-uclibcgnueabihf --with-ssl=openssl --with-openssl=yes LDFLAGS=-L/home/samba_share/yuanbo/wget/_install/lib CPPFLAGS=-I/home/samba_share/yuanbo/wget/_install/include --enable-shared=no
```

源码路径：wget2-2.0.1版本编译存在问题
https://ftp.gnu.org/gnu/wget/