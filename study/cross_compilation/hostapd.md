脚本：
```bash
#libnl-3.7.0
./configure --prefix=/home/samba_share/yuanbo/hostapd/_install --host=arm-rockchip830-linux-uclibcgnueabihf --enable-static
make
make install


#openssl
vim Makefile //删除所有-m64
./Configure no-asm no-shared no-async linux-generic32 --prefix=/home/samba_share/yuanbo/hostapd/_install --cross-compile-prefix=arm-rockchip830-linux-uclibcgnueabihf-
make
make install

cp deconfig .config
vim Makefile
#Makefile中添加以下三行
#LDFLAGS += -static
#CFLAGS += -I/home/samba_share/yuanbo/hostapd/_install/include/libnl3
#CFLAGS += -I/home/samba_share/yuanbo/hostapd/_install/include
#LIBS += -L/home/samba_share/yuanbo/hostapd/_install/lib -lnl-3 -lcrypto -lssl
make CC=arm-rockchip830-linux-uclibcgnueabihf-gcc STATIC=1
```
资料：
https://blog.csdn.net/Turix/article/details/120993636

hostapd.conf 释义

https://blog.csdn.net/beibei\_xiansen/article/details/127739225