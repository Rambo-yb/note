一、内核添加驱动支持
![image](images/_y0iflAHDhC3Mm7uAlddl8jKL-SN0ocIpPOkaRB9XyE.png)

二、查看摄像头信息
使用命令`v4l2-ctl --list-devices`查看设备节点中是否存在新增的USB摄像头，注意usb摄像头先接入设备再给设备上电，RK平台默认不支持usb摄像头热插拔功能
![image](images/pvSVSh68XDP_RIQMONTGOFEVYG9bcyo5ctndSOp9x0o.png)

使用命令`v4l2-ctl --list-formats-ext -d /dev/video45`查看设备节点支持的像素格式、分辨率、刷新频率信息
![image](images/ju5HZzefHOG_Ah1vkt3TL6YE0ov4h-7jtdTHPdrIPe4.png)



