NFS Server

##### Ubuntu 20.04

On server side

Install package
```
apt install nfs-kernel-server -y
```

Create directory and permision

```
mkdir /mnt/nfs
chown nobody:nogroup /mnt/nfs
chmod 777 /mnt/nfs
```
Add mount to export

```
vim /etc/exports

/mnt/nfs       192.168.1.0/24(rw,sync,no_subtree_check)
```
```
exportfs -r
exportfs -a
```
Restart service

```
systemctl restart nfs-kernel-server
```

```
ufw disable
# for add roule if ufw enable

ufw allow from 192.168.1.1/24 to any port nfs

```
##### On clients side

```
apt update
apt install nfs-common -y
```

Create directroy

```
mkdir /mnt/client

mount 192.168.1.180:/mnt/nfs /mnt/client

```

Thats all, create file on server and client directory

