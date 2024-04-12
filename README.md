
# Linux Tricks
1. [Disable Ipv6 on CentOS 8](https://fmottamendes.github.io/linux_tricks/#disable-ipv6-on-centos-8)
2. [LVM Extend](https://fmottamendes.github.io/linux_tricks/#lvm-extend)
3. [NO_PUBKEY](https://fmottamendes.github.io/linux_tricks/#no_pubkey)

## Disable Ipv6 on CentOS 8
**Create disableipv6.conf file on /etc/sysctl.d directory:**
```
vi /etc/sysctl.d/disableipv6.conf
```
**Add this next 2 lines in disableipv6.conf:**
```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
```
**If you need disable Ipv6 only in one interface use the next line arg(*enp0s3 in example*):**
```
net.ipv6.conf.enp0s3.disable_ipv6 = 1
```
**Restart sysctl service:**
```
systemctl restart systemd-sysctl
```

## LVM Extend
**Create partition 8e type with fdisk.**

**Create a physical volume:**
```
pvcreate /dev/sdXx
```
**Add physical volumes to a volume group:**
```
vgextend vg_name /dev/sdXx
```
**Extend a logical volume:** (*use 100% free space of volume group to logical volume*)
```
lvextend -l +100%FREE /dev/vg_name/volume_name
```
(*define the new size of logical volume to 542Gb*)
```
lvextend -L 542G /dev/vg_name/volume_name
```
(*add 1Gb to logical volume*)
```
lvextend -L +1G /dev/vg_name/volume_name
```
**Increase de file system size:** (*for XFS file system*)
```
xfs_growfs /mount/point -D size - xfs_growfs /mount/point
```
(*for EXT file system*)
```
resize2fs /mount/point
```

## NO_PUBKEY
**Error NO_PUBKEY**

"The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 5BA31D57EF5975CA"
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
```
