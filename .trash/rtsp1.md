# rtsp
##### 版本迭代
| 版本     | 日期         | 内容                                                                       |
| ------ | ---------- | ------------------------------------------------------------------------ |
| V1.0.0 | 2025.03.10 | 1. 设备端多路视频流，通过不同的url同时预览<br>2. 音视频同时传输<br>3. 视频格式H264/H265，音频格式AAC/G711A |
|        |            |                                                                          |
|        |            |                                                                          |

##### URL定义
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
码流类型：【1：主码流】【2：子码流】【3：第三码流】
回放视频流：同一时间只允许一个用户预览回放录像
```