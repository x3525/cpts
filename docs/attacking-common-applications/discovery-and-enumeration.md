# Discovery and Enumeration

## Hosts File

```sh
my@attack:~$ echo "10.129.170.73 $(tr '\n' ' ' < scope)" | sudo tee -a /etc/hosts
```

## Nmap

```sh
my@attack:~$ sudo nmap -iL scope -p- --open -A -oA results
```

```output title="Output" hl_lines="2 27 58 85 116 148 180 212 244 270 296 322"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-05 23:39 +03
Nmap scan report for app-dev.inlanefreight.local (10.129.170.73)
Host is up (0.19s latency).
Not shown: 64087 closed tcp ports (reset), 1446 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Testing Default Vhosts
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for app.inlanefreight.local (10.129.170.73)
Host is up (0.12s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 64248 closed tcp ports (reset), 1285 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-robots.txt: 15 disallowed entries
| /joomla/administrator/ /administrator/ /bin/ /cache/
| /cli/ /components/ /includes/ /installation/ /language/
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-title: Home
|_http-generator: Joomla! - Open Source Content Management
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for blog.inlanefreight.local (10.129.170.73)
Host is up (0.15s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 62320 closed tcp ports (reset), 3213 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-generator: WordPress 5.8
|_http-title: Inlanefreight Blog
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for dev.inlanefreight.local (10.129.170.73)
Host is up (0.12s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 61934 closed tcp ports (reset), 3599 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-generator: Joomla! - Open Source Content Management
| http-robots.txt: 15 disallowed entries
| /joomla/administrator/ /administrator/ /bin/ /cache/
| /cli/ /components/ /includes/ /installation/ /language/
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-title: Home
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for drupal-acc.inlanefreight.local (10.129.170.73)
Host is up (0.16s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 63118 closed tcp ports (reset), 2415 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-generator: Drupal 7 (http://drupal.org)
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Drupal ACC Environment
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt
|_/LICENSE.txt /MAINTAINERS.txt
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for drupal-dev.inlanefreight.local (10.129.170.73)
Host is up (0.15s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 62771 closed tcp ports (reset), 2762 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Home | Inlanefreight Dev
|_http-generator: Drupal 8 (https://www.drupal.org)
| http-robots.txt: 22 disallowed entries (15 shown)
| /core/ /profiles/ /README.txt /web.config /admin/
| /comment/reply/ /filter/tips/ /node/add/ /search/ /user/register/
| /user/password/ /user/login/ /user/logout/ /index.php/admin/
|_/index.php/comment/reply/
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for drupal-qa.inlanefreight.local (10.129.170.73)
Host is up (0.14s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 63827 closed tcp ports (reset), 1706 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt
|_/LICENSE.txt /MAINTAINERS.txt
|_http-title: drupal-qa.inlanefreight.local
|_http-generator: Drupal 7 (http://drupal.org)
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for drupal.inlanefreight.local (10.129.170.73)
Host is up (0.16s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 64853 closed tcp ports (reset), 680 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-generator: Drupal 8 (https://www.drupal.org)
| http-robots.txt: 22 disallowed entries (15 shown)
| /core/ /profiles/ /README.txt /web.config /admin/
| /comment/reply/ /filter/tips /node/add/ /search/ /user/register/
| /user/password/ /user/login/ /user/logout/ /index.php/admin/
|_/index.php/comment/reply/
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Home | Inlanefreight Blog
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for gitlab.inlanefreight.local (10.129.170.73)
Host is up (0.15s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 64369 closed tcp ports (reset), 1164 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Testing Default Vhosts
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for jenkins.inlanefreight.local (10.129.170.73)
Host is up (0.15s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 65083 closed tcp ports (reset), 450 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Testing Default Vhosts
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for support.inlanefreight.local (10.129.170.73)
Host is up (0.15s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 62379 closed tcp ports (reset), 3154 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Testing Default Vhosts
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

Nmap scan report for web01.inlanefreight.local (10.129.170.73)
Host is up (0.14s latency).
rDNS record for 10.129.170.73: app-dev.inlanefreight.local
Not shown: 63889 closed tcp ports (reset), 1644 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Testing Default Vhosts
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT       ADDRESS
1   264.90 ms 10.10.14.1
2   266.05 ms app-dev.inlanefreight.local (10.129.170.73)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 12 IP addresses (12 hosts up) scanned in 1055.98 seconds
```

## [EyeWitness](https://github.com/RedSiege/EyeWitness)

```sh
my@attack:~$ EyeWitness.py --timeout 30 --web -x results.xml -d EyeWitness_Report
```

```output title="Output"
################################################################################
#                                  EyeWitness                                  #
################################################################################
#           Red Siege Information Security - https://www.redsiege.com          #
################################################################################

Starting Web Requests (12 Hosts)
Attempting to screenshot http://app-dev.inlanefreight.local
Attempting to screenshot http://app.inlanefreight.local
Attempting to screenshot http://blog.inlanefreight.local
Attempting to screenshot http://dev.inlanefreight.local
Attempting to screenshot http://drupal-acc.inlanefreight.local
Attempting to screenshot http://drupal-dev.inlanefreight.local
Attempting to screenshot http://drupal-qa.inlanefreight.local
Attempting to screenshot http://drupal.inlanefreight.local
Attempting to screenshot http://gitlab.inlanefreight.local
Attempting to screenshot http://jenkins.inlanefreight.local
Attempting to screenshot http://support.inlanefreight.local
Attempting to screenshot http://web01.inlanefreight.local
Finished in 30.359581232070923 seconds
```

## [Aquatone](https://github.com/shelld3v/aquatone)

```sh
my@attack:~$ aquatone -timeout 30000 -nmap -input-file results.xml -out Aquatone_Report
```

```output title="Output"
aquatone v1.9.2-shelld3v started at 2025-05-06T00:41:48+03:00

 :: Targets          : 36
 :: Threads          : 8
 :: Ports            : 80, 443, 8080, 8443
 :: Output Directory : Aquatone_Report

http://10.129.170.73/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://10.129.170.73/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://app.inlanefreight.local/: 200 OK
http://10.129.170.73/: 200 OK
http://10.129.170.73/: 200 OK
http://10.129.170.73/: 200 OK
http://support.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://drupal.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://blog.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://10.129.170.73/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://web01.inlanefreight.local/: 200 OK
http://10.129.170.73/: 200 OK
http://drupal-dev.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://dev.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://10.129.170.73/: 200 OK
http://10.129.170.73/: 200 OK
http://gitlab.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: 200 OK
http://10.129.170.73/: 200 OK
http://drupal-qa.inlanefreight.local/: 200 OK
http://10.129.170.73/: 200 OK
http://10.129.170.73/: 200 OK
http://jenkins.inlanefreight.local/: 200 OK
http://drupal-acc.inlanefreight.local/: 200 OK
http://app-dev.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://support.inlanefreight.local/: screenshot successful
http://app.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://drupal.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://web01.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://drupal-dev.inlanefreight.local/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://app-dev.inlanefreight.local/: screenshot successful
http://dev.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://gitlab.inlanefreight.local/: screenshot successful
http://10.129.170.73/: screenshot successful
http://10.129.170.73/: screenshot successful
http://10.129.170.73/: screenshot successful
http://jenkins.inlanefreight.local/: screenshot successful
http://drupal-qa.inlanefreight.local/: screenshot successful
http://drupal-acc.inlanefreight.local/: screenshot successful
http://blog.inlanefreight.local/: screenshot successful
Calculating page structures... done
Clustering similar pages... done
Generating HTML report... done

Writing session file...Time:
 - Started at  : 2025-05-06T00:41:48+03:00
 - Finished at : 2025-05-06T00:42:16+03:00
 - Duration    : 29s

Requests:
 - Successful : 36
 - Failed     : 0

 - 2xx : 36
 - 3xx : 0
 - 4xx : 0
 - 5xx : 0

Screenshots:
 - Successful : 36
 - Failed     : 0

Wrote HTML report to: Aquatone_Report/aquatone_report.html
```
