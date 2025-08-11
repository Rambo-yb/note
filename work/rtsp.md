
| 版本     | 修改内容           | 日期         | 代码版本 |
| ------ | -------------- | ---------- | ---- |
| V1.0.0 | 定义rtsp url基本格式 | 2025.02.11 |      |
|        |                |            |      |
|        |                |            |      |
##### 一、方案调研
| 方案           | 因素                                                          |
| ------------ | ----------------------------------------------------------- |
| LIVE555      | 与1126SDK版本冲突，导致使用SDK程序编译存在问题，且官网无法下载老版本源码                   |
| gstreamer    | 环境搭建复杂，构建方式python版本要求3.7，但ubuntu中常用python3.6版，使用3.7版本会有其他异常 |
| RtspServer   | 相较于LIVE555实现简洁易懂，根据功能需求修改难度低；但支持的类型较少，需要自行添加，tcp预览有问题       |
| jrtplib      | 需要自行实现rtcp通讯流程                                              |
| media-server |                                                             |
| librtsp      |                                                             |
##### 二、URL定义
```Plain Text
1.实时视频流
rtsp://<ip>:<port>/streaming/<id>?auth=<username>:<password>&<param>
<ip>:设备ip
<port>:rtsp端口
<id>:通道号 通道*100+码流类型
<username>:用户名(预留)，默认:admin
<password>:密码(预留)，默认:123456
<param>:扩展内容，便于后期扩展

2.回放视频流
rtsp://<ip>:<port>/recording/<id>?auth=<username>:<password>&<param>
<id>:通道号 通道*100+1
<param>:
1) record_file=<filename>&offset_time=<second>:通过指定文件进行录像回放,offset_time：从文件开始时间多少秒开始播放
2) start_time=<time>&end_time=<time>:通过指定时间段进行录像回放，<time>:时间 20250220T000159

注：
码流类型：【1：主码流】【2：子码流】
回放视频流：同一时间只允许一个用户预览回放录像，
```