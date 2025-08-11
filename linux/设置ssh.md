##### 一.ssh链接
1.安装openssh-server
`apt update`
`apt install openssh-server`

2.修改openssh配置
`vim /etc/ssh/sshd_config`
```cpp
...
Port 22
...
PermitRootLogin yes
...
```
3.启动ssh
`/etc/init.d/ssh stop`
`/etc/init.d/ssh start`
`service ssh restart`
设置默认启动`systemctl enable ssh`

##### 二.创建root用户
1.设置root密码
`passwd root`

2.修改50-ubuntu.conf
`vim /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf`
文件末尾增加：
```cpp
...
greeter-show-manual-login=true
allow-guest=false
```

3.修改gdm-autologin
`vim /etc/pam.d/gdm-autologin`
注释内容：
```cpp
...
#auth	required	pam_succeed_if.so user != root quiet_success
...
```

4.修改gdm-password
`vim /etc/pam.d/gdm-password`

注释内容：
```cpp
...
#auth	required	pam_succeed_if.so user != root quiet_success
...
```

5.修改.profile
`vim /root/.profile`

替换内容：
```cpp
...
#mesg n || true
tty -s && mesg n || true
```

参考：
[Linux虚拟机配置ssh远程连接详细步骤(保姆级教程)\_虚拟机安装ssh-CSDN博客](https://blog.csdn.net/m0_64655190/article/details/130569010)
[ubuntu20.04 使用root用户登录系统\_ubuntu 20.04 root登录-CSDN博客](https://blog.csdn.net/COCO56/article/details/107628019)