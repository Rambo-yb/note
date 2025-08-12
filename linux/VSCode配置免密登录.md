##### 1.配置remote-ssh
修改.ssh/config文件
![[vscode配置远端主机.png]]
##### 2.配置密钥
使用cmd命令：`ssh-keygen`，在用户目录生成密钥文件
将id_rsa.pub，复制到虚拟机~/.ssh目录下，重命名为authorized_keys
![[密钥文件路径.png]]