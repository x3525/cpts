# Environment Enumeration

## Operating System

```shell
htb-student@ubuntu~$ cat /etc/os-release
```

```ini title="Output" hl_lines="5"
NAME="Ubuntu"
VERSION="20.04.3 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.3 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```

## PATH

```sh
htb-student@ubuntu~$ echo $PATH
```

```output title="Output"
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

## Environment Variables

```sh
htb-student@ubuntu:~$ env
```

```ini title="Output"
USER=htb-student
XDG_SESSION_TYPE=tty
HOME=/home/htb-student
MOTD_SHOWN=pam
DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1001/bus
LOGNAME=htb-student
XDG_SESSION_CLASS=user
TERM=alacritty
XDG_SESSION_ID=1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
XDG_RUNTIME_DIR=/run/user/1001
LANG=en_US.UTF-8
SHELL=/bin/sh
PWD=/home/htb-student
XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop
```

## Kernel Information

```sh
htb-student@ubuntu:~$ uname -a
```

```output title="Output"
Linux ubuntu 5.4.0-148-generic #165-Ubuntu SMP Tue Apr 18 08:53:12 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
```

## CPU

```sh
htb-student@ubuntu:~$ lscpu
```

```output title="Output"
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   43 bits physical, 48 bits virtual
CPU(s):                          2
On-line CPU(s) list:             0,1
Thread(s) per core:              1
Core(s) per socket:              1
Socket(s):                       2
NUMA node(s):                    1
Vendor ID:                       AuthenticAMD
CPU family:                      25
Model:                           1
Model name:                      AMD EPYC 7763 64-Core Processor
Stepping:                        1
CPU MHz:                         2445.405
BogoMIPS:                        4890.81
Hypervisor vendor:               VMware
Virtualization type:             full
L1d cache:                       64 KiB
L1i cache:                       64 KiB
L2 cache:                        1 MiB
L3 cache:                        512 MiB
NUMA node0 CPU(s):               0,1
Vulnerability Itlb multihit:     Not affected
Vulnerability L1tf:              Not affected
Vulnerability Mds:               Not affected
Vulnerability Meltdown:          Not affected
Vulnerability Mmio stale data:   Not affected
Vulnerability Retbleed:          Not affected
Vulnerability Spec store bypass: Vulnerable
Vulnerability Spectre v1:        Mitigation; usercopy/swapgs barriers and __user pointer sanitization
Vulnerability Spectre v2:        Mitigation; Retpolines, IBPB conditional, STIBP disabled, RSB filling, PBRSB-eIBRS Not affected
Vulnerability Srbds:             Not affected
Vulnerability Tsx async abort:   Not affected
Flags:                           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 syscall nx mmxext fxsr_opt pdpe1gb rdtscp
                                  lm constant_tsc rep_good nopl tsc_reliable nonstop_tsc cpuid extd_apicid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popc
                                 nt aes xsave avx f16c rdrand hypervisor lahf_lm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw invpcid_single ibpb vmmcall fsg
                                 sbase bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb sha_ni xsaveopt xsavec xsaves clzero arat pku ospke overflow_recov s
                                 uccor
```

## Login Shells

```sh
htb-student@ubuntu:~$ cat /etc/shells
```

```output title="Output" hl_lines="9 10"
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
```

## Routing Table

```sh
htb-student@ubuntu:~$ route
```

```output title="Output"
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         10.129.0.1      0.0.0.0         UG    0      0        0 ens160
10.129.0.0      0.0.0.0         255.255.0.0     U     0      0        0 ens160
```

## ARP Table

```sh
htb-student@ubuntu:~$ arp -a
```

```output title="Output"
? (10.129.0.1) at 00:50:56:b9:e9:28 [ether] on ens160
```

## DNS

```sh
htb-student@ubuntu:~$ cat /etc/resolv.conf
```

```output title="Output"
# This file is managed by man:systemd-resolved(8). Do not edit.
#
# This is a dynamic resolv.conf file for connecting local clients to the
# internal DNS stub resolver of systemd-resolved. This file lists all
# configured search domains.
#
# Run "resolvectl status" to see details about the uplink DNS servers
# currently in use.
#
# Third party programs must not access this file directly, but only through the
# symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a different way,
# replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 127.0.0.53
options edns0 trust-ad
```

## Existing Users

```sh
htb-student@ubuntu:~$ cat /etc/passwd
```

```linuxconfig title="Output" hl_lines="1 33 35"
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
usbmux:x:111:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
sshd:x:112:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
lab_adm:x:1000:1000:lab_adm:/home/lab_adm:/bin/bash
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
htb-student:x:1001:1001::/home/htb-student:/bin/sh
```

## Existing Groups

```sh
htb-student@ubuntu:~$ cat /etc/group
```

```linuxconfig title="Output" hl_lines="21"
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,lab_adm
tty:x:5:syslog
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:
fax:x:21:
voice:x:22:
cdrom:x:24:lab_adm
floppy:x:25:
tape:x:26:
sudo:x:27:lab_adm
audio:x:29:
dip:x:30:lab_adm
www-data:x:33:
backup:x:34:
operator:x:37:
list:x:38:
irc:x:39:
src:x:40:
gnats:x:41:
shadow:x:42:
utmp:x:43:
video:x:44:
sasl:x:45:
plugdev:x:46:lab_adm
staff:x:50:
games:x:60:
users:x:100:
nogroup:x:65534:
systemd-journal:x:101:
systemd-network:x:102:
systemd-resolve:x:103:
systemd-timesync:x:104:
crontab:x:105:
messagebus:x:106:
input:x:107:
kvm:x:108:
render:x:109:
syslog:x:110:
tss:x:111:
uuidd:x:112:
tcpdump:x:113:
ssh:x:114:
landscape:x:115:
lxd:x:116:lab_adm
systemd-coredump:x:999:
lab_adm:x:1000:
netdev:x:117:
htb-student:x:1001:
```

## Home Directory

```sh
htb-student@ubuntu:~$ ls /home
```

```output title="Output"
htb-student  lab_adm
```

## Mounted File Systems

```sh
htb-student@ubuntu:~$ df -h
```

```output title="Output"
Filesystem                         Size  Used Avail Use% Mounted on
udev                               1.9G     0  1.9G   0% /dev
tmpfs                              394M  1.1M  393M   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv  7.4G  3.8G  3.6G  52% /
tmpfs                              2.0G     0  2.0G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
tmpfs                              2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/sda2                          345M  254M   65M  80% /boot
/dev/loop2                          64M   64M     0 100% /snap/core20/1891
/dev/loop3                          54M   54M     0 100% /snap/snapd/19122
/dev/loop1                          56M   56M     0 100% /snap/core18/2751
/dev/loop0                          56M   56M     0 100% /snap/core18/2745
/dev/loop5                          74M   74M     0 100% /snap/core22/750
/dev/loop4                          74M   74M     0 100% /snap/core22/634
/dev/loop6                          54M   54M     0 100% /snap/snapd/19361
/dev/loop7                          62M   62M     0 100% /snap/core20/1169
/dev/loop8                         167M  167M     0 100% /snap/lxd/24846
/dev/loop9                         171M  171M     0 100% /snap/lxd/24918
tmpfs                              394M     0  394M   0% /run/user/1001
```

## Unmounted File Systems

```sh
htb-student@ubuntu:~$ grep -v '#' /etc/fstab | column -t
```

```output title="Output"
/dev/disk/by-id/dm-uuid-LVM-zoHZXlYbLjLFMhq6Aiv0porczzq5I1KtfvSvv1v2Taf4fTvbOuSWdaCoIPK07t2M  /      ext4  defaults  0  0
/dev/disk/by-uuid/729deb8f-f6f4-4f20-9aa7-f6baecf003a1                                        /boot  ext4  defaults  0  0
```

## Temp

```sh
htb-student@ubuntu:~$ ls -l /tmp /var/tmp /dev/shm
```

## Hidden Files

```sh
htb-student@ubuntu:~$ find / -name ".*" -type f -ls 2> /dev/null
```

## Hidden Directories

```sh
htb-student@ubuntu:~$ find / -name ".*" -type d -ls 2> /dev/null
```

## Writable Files

```sh
htb-student@ubuntu:~$ find / -path /proc -prune -o -type f -perm /0002 2> /dev/null
```

## Writable Directories

```sh
htb-student@ubuntu:~$ find / -path /proc -prune -o -type d -perm /0002 2> /dev/null
```
