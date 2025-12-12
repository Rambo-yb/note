#### 什么是MQTT
MQTT（Message Queuing Telemetry Transport）是一种轻量级、基于发布-订阅模式的消息传输协议，适用于资源受限的设备和低带宽、高延迟或不稳定的网络环境。它在物联网应用中广受欢迎，能够实现传感器、执行器和其它设备之间的高效通信。
#### 工作原理
- client1订阅topic1，client2订阅topic1，device订阅topic2，broker维护topic1/2/3
- client1发布topic2，broker将收到的topic2消息转发到device；client2发布topic3同理
- device发布topic1，client1/2都订阅了topic1，所以broker将topic1消息分别转发给client1/2
![[MQTT工作原理.svg]]
#### 工作流程
##### 客户端
- 使用TCP/IP协议与Broker建立连接
- 向Broker订阅需要接收消息的主题
- 接收订阅主题消息、发布特定主题消息
- 取消订阅、断开连接
##### Broker(服务器)
- 使用TCP/IP协议监听是否有客户端连接
- 记录客户端订阅的主题信息
- 接收客户端发布的消息，并根据主题信息转发给订阅了该主题的客户端
#### 协议文档
[[MQTT_协议_3.1.1_中文版.pdf]]
[[mqtt-v5.0-zh_cn.pdf]]
