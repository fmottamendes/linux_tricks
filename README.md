
# Linux Tricks
1. [Disable Ipv6 on CentOS 8](https://fmottamendes.github.io/linux_tricks/#disable-ipv6-on-centos-8)
2. [LVM Extend](https://fmottamendes.github.io/linux_tricks/#lvm-extend)

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

## LVM Extend
Create partition 8e type with fdisk
Create a physical volume:
```
pvcreate /dev/sdXx
```
Add physical volumes to a volume group:
```
vgextend vg_name /dev/sdXx
```
Extend a logical volume:
```
lvextend -l +100%FREE /dev/vg_name/volume_name
```
(*use 100% free space of volume group to logical volume*)

```
lvextend -L 542G /dev/vg_name/volume_name
```
(*define the new size of logical volume*)
```
lvextend -L +1G /dev/vg_name/volume_name
```
(*add 1Gb to logical volume*)

Increase de file system size:
```
xfs_growfs /mount/point -D size - xfs_growfs /mount/point
```
(*for XFS file system*)
```
resize2fs /mount/point
```
(*for EXT file system*)
