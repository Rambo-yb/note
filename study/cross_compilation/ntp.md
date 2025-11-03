脚本：
```bash
#!/bin/sh

set -e

[ ! $INSTALL_ROOT ] && exit -1

CUR_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

# 安装路径
INSTALL_DIR=$INSTALL_ROOT/ntp

# 源码包
pkg_with_ver=ntp-4.2.8p17
pkg_tar_file=$pkg_with_ver.tar.gz
[ "$1" == "rebuild" ] && rm -rf $pkg_with_ver
[ "$1" == "clean" ] && rm -rf $pkg_with_ver && exit

# 配置命令
if [[ $DEVICE = "x86_64" ]]
then
	cmd="./configure CFLAGS=\"-O2 -g -fPIC\" --prefix=$INSTALL_DIR CC=gcc --with-openssl-incdir=$INSTALL_ROOT/openssl/include --with-openssl-libdir=$INSTALL_ROOT/openssl/lib --with-ntpsnmpd=no --with-yielding-select=yes"
else
	#cmd="./configure CFLAGS=\"-O2 -g -fPIC\" --prefix=$INSTALL_DIR --host=$HOST CC=$HOST-gcc --with-openssl-incdir=$INSTALL_ROOT/openssl/include --with-openssl-libdir=$INSTALL_ROOT/openssl/lib --with-ntpsnmpd=no --with-yielding-select=yes"
	cmd="./configure CFLAGS=\"-O2 -g -fPIC\" --prefix=$INSTALL_DIR --host=$HOST CC=$HOST-gcc --with-ntpsnmpd=no --with-yielding-select=yes  --with-openssl-incdir=$INSTALL_ROOT/openssl/include --with-crypto=$INSTALL_ROOT/openssl/lib/libcrypto.a"
fi
# 编译命令
build_lib()
{
	cd $CUR_DIR
	[ -d $pkg_with_ver ] && echo "builded" && return 0
        rm -rf $pkg_with_ver $INSTALL_DIR
        tar xzvf $pkg_tar_file
        cd $pkg_with_ver
	eval $cmd
	make
    make install
}

# 执行编译
echo $DEVICE" building "$pkg_with_ver
if [[ $DEVICE = "rv1106" ]]
then
	build_lib
elif [[ $DEVICE = "fh8852" ]]
then
	echo "fh8852 do nothing"
else
	build_lib
fi
```
源码下载路径 http://www.ntp.org/downloads/