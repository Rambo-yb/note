1.编辑 resolved.conf
```bash
vi /etc/systemd/resolved.conf
```
##### 2.取消注释并修改 DNS 设置
```bash
[Resolve]
DNS=8.8.8.8  # 取消注释并设置 DNS
#FallbackDNS=     # 建议注释此行
Domains=~.        # 取消注释以支持所有域名
```
##### 3.重启服务并更新链接
```bash
systemctl restart systemd-resolved
ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```