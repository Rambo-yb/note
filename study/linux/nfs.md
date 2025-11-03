1.配置nfs
`apt install nfs-kernel-server`

`mkdir -p /home/smb/work
`chmod 777 /home/smb/work -R

`vim /etc/exports`
最后一行添加：
```
...
/home/nfs_share *(rw,sync,no_root_squash,no_subtree_check)

```

`/etc/init.d/nfs-kernel-server restart`

[linux 安装配置NFS服务器 - 浇筑菜鸟 - 博客园](https://www.cnblogs.com/jzcn/p/14808681.html)

2.virtual box 给虚拟机添加网卡桥接
![image](images/p6znAwDjp1DXMdd-CP6jDgI3lWy8Fqwk1FaJyNmzDrc.png)

3修改./etc/netplan/00-installer-config.yaml，设置合适的网卡ip
![image](images/ATDI0c-5mp1XXuvmUFjT8Wiq3r0mqw6Cj4vR_Dp4WQY.png)

4.设备端挂载命令
`mount -t nfs -o nolock 192.168.5.20:/home/samba_share/nfsdir /root`