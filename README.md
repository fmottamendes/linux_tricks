
# Linux Tricks
1. [Disable Ipv6 on CentOS 8](https://fmottamendes.github.io/linux_tricks/#disable-ipv6-on-centos-8)
2. [LVM Extend](https://fmottamendes.github.io/linux_tricks/#lvm-extend)
3. [apt update NO_PUBKEY](https://fmottamendes.github.io/linux_tricks/#apt-update-no_pubkey)
4. [AKS from az CLI](https://fmottamendes.github.io/linux_tricks/#aks-from-az-cli)
5. [Add user to argoCD](https://fmottamendes.github.io/linux_tricks/#add-user-to-argocd)

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

## apt update NO_PUBKEY
**Error NO_PUBKEY**

"The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 5BA31D57EF5975CA"
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
```

## AKS from az CLI
```
az aks command invoke --resource-group nome-rg --name nome_aks --command "kubectl get pods -A"
```

## Add user to argoCD
```
kubectl get cm argocd-cm -n=argocd -o yaml > argocd-cm.yaml
kubectl get cm argocd-rbac-cm -n=argocd -o yaml > argocd-rbac-cm.yaml
```
** Add this to argocd-cm.yaml file
```
apiVersion: v1
data:
  accounts.nome_usuario: login
  exec.enabled: "true"
kind: ConfigMap
metadata:
  labels:
    ...
```
** Add this to argocd-rbac-cm.yaml file
```
apiVersion: v1
data:
  policy.csv: |
    p, role:readonly-user, applications, get, default/myapp-*, allow
    g, nome_usuario, role:readonly-user
kind: ConfigMap
metadata:
  labels:
  ...
```
** Apply the changes
```
kubectl apply -f argocd-cm.yaml
kubectl apply -f argocd-rbac-cm.yaml
```
** Log in argoCD CLI
```
argocd login argocd.xyz.com:443 --username admin --password test@123
```
** List accounts and define password for new user
```
argocd account list
argocd account update-password --account nome_user --new-password test@1#
```
