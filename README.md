#Linux Tricks
## Disable Ipv6 on CentOS 8
Create disableipv6.conf file on /etc/sysctl.d directory:
```
vi /etc/sysctl.d/disableipv6.conf
```
Add this next 2 lines in disableipv6.conf:
```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
```
If you need disable Ipv6 only in one interface use the next line arg(*enp0s3 in example*):
```
net.ipv6.conf.enp0s3.disable_ipv6 = 1
```
Restart sysctl service:
```
systemctl restart systemd-sysctl
```
