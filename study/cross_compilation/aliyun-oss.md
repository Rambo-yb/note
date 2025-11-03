```bash
#!/bin/bash

BASE_PATH=`pwd`
OUTPUT_PATH=${BASE_PATH}/aliyun-oss

export PATH="/home/samba_share/rv1126_rv1109/prebuilts/gcc/linux-x86/arm/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin:$PATH"

#交叉编译curl
tar -vxf curl-7.86.0.tar.xz
cd ./curl-7.86.0
./configure --prefix=${OUTPUT_PATH} --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc --without-ssl
make
make install
cd ${BASE_PATH}
rm curl-7.86.0 -r

#交叉编译apr
tar -vxf apr-1.7.0.tar.gz
cd ./apr-1.7.0
touch libtoolT
./configure
make
cp ./tools/gen_test_char ../
make clean
touch libtoolT
./configure --prefix=${OUTPUT_PATH} --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc ac_cv_file__dev_zero=yes ac_cv_func_setpgrp_void=yes apr_cv_tcp_nodelay_with_cork=yes ac_cv_sizeof_struct_iovec=8
cp -a ../gen_test_char ./tools/
#note：此处需要将Makefile第134行注释 "OBJECTS_gen_test_char = tools/gen_test_char.lo $(LOCAL_LIBS)" --> "#OBJECTS_gen_test_char = tools/gen_test_char.lo $(LOCAL_LIBS)"，目前没有更好的解决方式 
vim Makefile +134
make
make install
rm ../gen_test_char
cd ${BASE_PATH}
rm ./apr-1.7.0 -r

#交叉编译expat
tar -vxf expat-2.2.7.tar.xz
cd ./expat-2.2.7
./configure --prefix=${OUTPUT_PATH} --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc
make
make install
cd ${BASE_PATH}
rm ./expat-2.2.7 -r

#交叉编译apr-util
tar -vxf apr-util-1.6.1.tar.gz
cd ./apr-util-1.6.1
./configure --prefix=${OUTPUT_PATH} --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc --with-apr=${OUTPUT_PATH} --with-expat=${OUTPUT_PATH}
make
make install
cd ${BASE_PATH}
rm ./apr-util-1.6.1 -r

#交叉编译mxml
tar -vxf mxml-release-2.9.tar.gz
cd ./mxml-release-2.9
./configure --prefix=${OUTPUT_PATH} --host=arm-linux-gnueabihf CC=arm-linux-gnueabihf-gcc 
#note：此处编译报错不影响使用，将头文件和库文件copy出去即可
make
cp *.h ${OUTPUT_PATH}/include/
cp libmxml.* ${OUTPUT_PATH}/lib/
cd ${BASE_PATH}
rm ./mxml-release-2.9 -r

#交叉编译sdk
tar -vxf aliyun-oss-c-sdk-3.5.0.tar.gz
cd ./aliyun-oss-c-sdk-3.5.0
#note：此处需要将CMakeLists.txt 124行和125行注释 "add_subdirectory(oss_c_sdk_sample)" --> "#add_subdirectory(oss_c_sdk_sample)"，"add_subdirectory(oss_c_sdk_test)" --> "#add_subdirectory(oss_c_sdk_test)"，这两个目录对于我们编译sdk没有意义，而且这里会有一些报错，所以直接注释掉。 
vim CMakeLists.txt +124
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_SYSTEM_NAME=linux -DCMAKE_SYSTEM_PROCESSOR=arm -DCMAKE_C_COMPILER=arm-linux-gnueabihf-gcc -DCMAKE_CXX_COMPILER=arm-linux-gnueabihf-g++ -DCMAKE_INSTALL_PREFIX=${OUTPUT_PATH} -DCURL_INCLUDE_DIR=${OUTPUT_PATH}/include/curl -DCURL_LIBRARY=${OUTPUT_PATH}/lib/libcurl.a -DAPR_INCLUDE_DIR=${OUTPUT_PATH}/include/apr-1 -DAPR_LIBRARY=${OUTPUT_PATH}/lib/libapr-1.a -DAPR_UTIL_INCLUDE_DIR=${OUTPUT_PATH}/include/apr-1 -DAPR_UTIL_LIBRARY=${OUTPUT_PATH}/lib/libaprutil-1.a -DMINIXML_INCLUDE_DIR=${OUTPUT_PATH}/include -DMINIXML_LIBRARY=${OUTPUT_PATH}/lib/libmxml.a -DAPR_CONFIG_BIN=${OUTPUT_PATH}/bin/apr-1-config -DAPU_CONFIG_BIN=${OUTPUT_PATH}/bin/apu-1-config -DCURL_CONFIG_BIN=${OUTPUT_PATH}/bin/curl-config ./
make
make install
cd ${BASE_PATH}
rm ./aliyun-oss-c-sdk-3.5.0 -r

cp ${OUTPUT_PATH}/lib ${BASE_PATH}/../../lib/aliyun-oss -rf
cp ${OUTPUT_PATH}/include ${BASE_PATH}/../../lib/aliyun-oss -rf
rm aliyun-oss -r
```
