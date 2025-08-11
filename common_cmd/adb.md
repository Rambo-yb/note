```shell
Z:\rv1126_rv1109\external\camera_engine_rkaiq\rkisp2x_tuner
.\adb.exe shell
进入设备shell命令行

.\adb.exe push Z:\yuanbo\FFmpeg userdata
Z:\yuanbo\FFmpeg 需要推送的文件
userdata 推送到设备的位置

.\adb.exe pull userdata E:\yuanbo\
userdata 需要拉取的目录
E:\yuanbo\ 拉取到电脑的位置

adb.exe push Z:\rv1126_rv1109\buildroot\output\rockchip_rv1126_rv1109\target\usr\bin\startup_app_lxt userdata
adb.exe pull userdata E:\yuanbo
```