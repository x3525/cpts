# Netfilter

## [CVE-2021-22555](https://github.com/google/security-research/blob/master/pocs/linux/cve-2021-22555/exploit.c)

| VULNERABLE KERNEL |
|---|
| 5.11 |

```sh
htb-student@ubuntu:~$ gcc -o exploit exploit.c
```

## [CVE-2022-25636](https://github.com/Bonfee/CVE-2022-25636)

| VULNERABLE KERNEL |
|---|
| 5.6.10 |

```sh
htb-student@ubuntu:~$ make -C CVE-2022-25636
```

## [CVE-2023-32233](https://github.com/Liuk3r/CVE-2023-32233)

| VULNERABLE KERNEL |
|---|
| 6.3.1 |

```sh
htb-student@ubuntu:~$ gcc -Wall -o exploit CVE-2023-32233/exploit.c -lmnl -lnftnl
```
