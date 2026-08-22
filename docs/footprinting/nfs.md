# NFS

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 111 | Open Network Computing Remote Procedure Call (ONC RPC, sometimes referred to as Sun RPC) |
| 2049 | Network File System (NFS) |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.202.5 -p 111,2049 -sV -sC --script "nfs*"
```

## Dangerous Settings

| OPTION |
|---|
| insecure |
| no_root_squash |
| nohide |
| rw |

## ExportFS

```sh
my@attack:~$ sudo exportfs
```

## Available Shares

```sh
my@attack:~$ sudo showmount -e 10.129.202.5
```

```output title="Output"
Export list for 10.129.202.5:
/var/nfs      10.0.0.0/8
/mnt/nfsshare 10.0.0.0/8
```

## Mount

```sh
my@attack:~$ sudo mount -t nfs 10.129.202.5:/ /mnt -o nolock
```

## Unmount

```sh
my@attack:~$ sudo umount /mnt
```
