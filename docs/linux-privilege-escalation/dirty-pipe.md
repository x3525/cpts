# Dirty Pipe

## Release

| VULNERABLE KERNEL |
|---|
| 5.17 |

```sh
htb-student@ubuntu:~$ uname -r
```

```output title="Output"
5.15.0-051500-generic
```

## [Exploitation](https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits/blob/main/exploit-1.c)

```sh
htb-student@ubuntu:~$ ./exploit-1
```

## [Exploitation](https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits/blob/main/exploit-2.c) (SUID)

```sh
htb-student@ubuntu:~$ ./exploit-2 /usr/bin/sudo
```
