# Linux Internals Enumeration

## Network Interfaces

```sh
htb-student@ubuntu:~$ ip a
```

```output title="Output" hl_lines="7"
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:50:56:b0:7c:a3 brd ff:ff:ff:ff:ff:ff
    inet 10.129.205.110/16 brd 10.129.255.255 scope global dynamic ens160
       valid_lft 2088sec preferred_lft 2088sec
    inet6 dead:beef::250:56ff:feb0:7ca3/64 scope global dynamic mngtmpaddr
       valid_lft 86395sec preferred_lft 14395sec
    inet6 fe80::250:56ff:feb0:7ca3/64 scope link
       valid_lft forever preferred_lft forever
```

## Hosts File

```sh
htb-student@ubuntu:~$ cat /etc/hosts
```

```output title="Output"
127.0.0.1 localhost
127.0.1.1 ubuntu

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

## Latest Logins

```sh
htb-student@ubuntu:~$ lastlog
```

```output title="Output" hl_lines="2 36"
Username         Port     From             Latest
root                                       Thu Jan  1 00:00:10 +0000 1970
daemon                                     **Never logged in**
bin                                        **Never logged in**
sys                                        **Never logged in**
sync                                       **Never logged in**
games                                      **Never logged in**
man                                        **Never logged in**
lp                                         **Never logged in**
mail                                       **Never logged in**
news                                       **Never logged in**
uucp                                       **Never logged in**
proxy                                      **Never logged in**
www-data                                   **Never logged in**
backup                                     **Never logged in**
list                                       **Never logged in**
irc                                        **Never logged in**
gnats                                      **Never logged in**
nobody                                     **Never logged in**
systemd-network                            **Never logged in**
systemd-resolve                            **Never logged in**
systemd-timesync                           **Never logged in**
messagebus                                 **Never logged in**
syslog                                     **Never logged in**
_apt                                       **Never logged in**
tss                                        **Never logged in**
uuidd                                      **Never logged in**
tcpdump                                    **Never logged in**
landscape                                  **Never logged in**
pollinate                                  **Never logged in**
usbmux                                     **Never logged in**
sshd                                       **Never logged in**
systemd-coredump                           **Never logged in**
lab_adm                                    **Never logged in**
lxd                                        **Never logged in**
htb-student      pts/0    10.10.14.235     Thu May 15 09:03:20 +0000 2025
```

## History

```sh
htb-student@ubuntu:~$ history
```

## Finding History Files

```sh
htb-student@ubuntu:~$ find / \( -name "*_hist" -o -name "*_history" \) -type f -ls 2> /dev/null
```

## Cron

```sh
htb-student@ubuntu:~$ ls -la /etc/cron.daily
```

```output title="Output"
drwxr-xr-x  2 root root 4096 Oct  6  2021 .
drwxr-xr-x 95 root root 4096 Jun  7  2023 ..
-rwxr-xr-x  1 root root  376 Dec  4  2019 apport
-rwxr-xr-x  1 root root 1478 Apr  9  2020 apt-compat
-rwxr-xr-x  1 root root  355 Dec 29  2017 bsdmainutils
-rwxr-xr-x  1 root root 1187 Sep  5  2019 dpkg
-rwxr-xr-x  1 root root  377 Jan 21  2019 logrotate
-rwxr-xr-x  1 root root 1123 Feb 25  2020 man-db
-rw-r--r--  1 root root  102 Feb 13  2020 .placeholder
-rwxr-xr-x  1 root root 4574 Jul 18  2019 popularity-contest
-rwxr-xr-x  1 root root  214 Apr  2  2020 update-notifier-common
```

## Binaries

```sh
htb-student@ubuntu:~$ ls -l /bin /usr/bin /usr/sbin
```

## Installed Packages

```sh
htb-student@ubuntu:~$ apt list --installed | tr '/' ' ' | cut -d ' ' -f 1,3 | sed 's/[0-9]://g' | tee -a installed.txt
```

## [GTFOBins](https://gtfobins.github.io/)

```sh
htb-student@ubuntu:~$ for bin in $(curl -s "https://gtfobins.github.io/" | html2text | cut -d ' ' -f 1 | grep -oP '(?<=\[)[^\]]+(?=\])'); do if grep -q "$bin" installed.txt; then echo "$bin"; fi; done
```

## Sudo Version

```sh
htb-student@ubuntu:~$ sudo -V
```

```output title="Output" hl_lines="1"
Sudo version 1.8.21p2
Sudoers policy plugin version 1.8.21p2
Sudoers file grammar version 46
Sudoers I/O plugin version 1.8.21p2
```

## Configuration Files

```sh
htb-student@ubuntu:~$ for ext in .conf .config .cnf; do echo -e "\nFile extension: $ext"; find / -name "*$ext" -type f 2> /dev/null | grep -v 'lib\|fonts\|share\|core'; done
```

## Scripts

```sh
htb-student@ubuntu:~$ for ext in .py .pyc .pl .go .jar .c .sh; do echo -e "\nFile extension: $ext"; find / -name "*$ext" -type f 2> /dev/null | grep -v 'doc\|lib\|headers\|share'; done
```

## Running Processes by Root

```sh
htb-student@ubuntu:~$ ps aux | grep root
```
