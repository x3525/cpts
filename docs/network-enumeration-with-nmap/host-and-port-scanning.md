# Host and Port Scanning

| STATE | DESCRIPTION |
|---|---|
| open | Port açık. |
| closed | Port kapalı. Dönen paket RST bayrağı içerir. |
| filtered | Port açık/kapalı durumu belirlenemiyor. Yanıt gelmiyor ya da hata kodu alınıyor. |
| unfiltered | Port açık/kapalı durumu belirlenemiyor. TCP ACK taraması sırasında ortaya çıkar. |
| open/filtered | Port yanıt vermiyor. Güvenlik duvarı veya paket filtresi mevcut olabilir. |
| closed/filtered | Port kapalı/korumalı durumu belirlenemiyor. TCP Idle taraması sırasında ortaya çıkar. |

## Discovering Open UDP Ports

!!! abstract

    [UDP taraması](https://nmap.org/book/scan-methods-udp-scan.html) yavaş bir taramadır. Çoğu zaman herhangi bir yanıt alınamaz.

| SCANNING OPTION | DESCRIPTION |
|---|---|
| -sU | UDP taraması. |
| -F | En popüler 100 port taranır. |

```sh
my@attack:~$ sudo nmap 10.129.2.47 -sU -F
```

```output title="Output"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 17:56 +03
Nmap scan report for 10.129.2.47
Host is up (0.20s latency).
Not shown: 96 closed udp ports (port-unreach)
PORT      STATE         SERVICE
68/udp    open|filtered dhcpc
137/udp   open          netbios-ns
138/udp   open|filtered netbios-dgm
49186/udp open|filtered unknown

Nmap done: 1 IP address (1 host up) scanned in 113.25 seconds
```

## Discovering Open TCP Ports

### Stealth Scan

!!! abstract

    [TCP SYN taraması](https://nmap.org/book/synscan.html) ayrıcalıklı kullanıcılar için varsayılan taramadır. Aksi belirtilmez ise en popüler 1000 port taranır.

```sh
my@attack:~$ sudo nmap 10.129.2.47 -sS --top-ports=10
```

```output title="Output"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-11 01:12 +03
Nmap scan report for 10.129.2.47
Host is up (0.15s latency).

PORT     STATE  SERVICE
21/tcp   closed ftp
22/tcp   open   ssh
23/tcp   closed telnet
25/tcp   closed smtp
80/tcp   open   http
110/tcp  open   pop3
139/tcp  open   netbios-ssn
443/tcp  closed https
445/tcp  open   microsoft-ds
3389/tcp closed ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 1.73 seconds
```

### Connect Scan

!!! abstract

    [TCP Connect taraması](https://nmap.org/book/scan-methods-connect-scan.html) gürültülü bir taramadır. Tamamlanmamış bağlantı veya gönderilmemiş paket bırakmaz.

```sh
my@attack:~$ sudo nmap 10.129.2.47 -sT
```

```output title="Output"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-11 01:13 +03
Nmap scan report for 10.129.2.47
Host is up (0.15s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
110/tcp   open     pop3
139/tcp   open     netbios-ssn
143/tcp   open     imap
445/tcp   open     microsoft-ds
5544/tcp  filtered unknown
8100/tcp  filtered xprint-server
27000/tcp filtered flexlm0
31337/tcp open     Elite
44442/tcp filtered coldfusion-auth

Nmap done: 1 IP address (1 host up) scanned in 47.28 seconds
```

### Trace the Packets

| SCANNING OPTION | DESCRIPTION |
|---|---|
| -p | Belirtilen port taranır. |
| -Pn | ICMP yankı isteklerini devre dışı bırak. |
| -n | DNS çözümlemesini devre dışı bırak. |
| --disable-arp-ping | ARP ping devre dışı bırak. |
| --packet-trace | Gönderilen ve alınan paketleri göster. |

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 21 -Pn -n --disable-arp-ping --packet-trace
```

```output title="Output" hl_lines="2 3"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 17:15 +03
SENT (0.0674s) TCP 10.10.14.229:58735 > 10.129.2.47:21 S ttl=42 id=29676 iplen=44  seq=1547310469 win=1024 <mss 1460>
RCVD (0.2478s) TCP 10.129.2.47:21 > 10.10.14.229:58735 RA ttl=63 id=0 iplen=40  seq=0 win=0
Nmap scan report for 10.129.2.47
Host is up (0.18s latency).

PORT   STATE  SERVICE
21/tcp closed ftp

Nmap done: 1 IP address (1 host up) scanned in 0.30 seconds
```

Çıktıya ait Request kısmını inceleyelim:

| MESSAGE | DESCRIPTION |
|---|---|
| SENT | Hedefe gönderilen paket. |
| TCP | Kullanılan protokol. |
| 10.10.14.229:58735 | Kaynak IPv4 ve kaynak port. |
| 10.129.2.47:21 | Hedef IPv4 ve hedef port. |
| S | TCP SYN bayrağı. |

Çıktıya ait Response kısmını inceleyelim:

| MESSAGE | DESCRIPTION |
|---|---|
| RCVD | Hedef tarafından gönderilen paket. |
| TCP | Kullanılan protokol. |
| 10.129.2.47:21 | Hedef IPv4 ve hedef port. |
| 10.10.14.229:58735 | Kaynak IPv4 ve kaynak port. |
| RA | TCP RST ve TCP ACK bayrağı. |

### Filtered Ports

!!! abstract

    Korumalı bir port yanıt vermez ise paket yeniden gönderilir. Bu durumda tarama daha uzun sürer.

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 139 -Pn -n --packet-trace
```

```output title="Output" hl_lines="8 10"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 17:54 +03
SENT (0.0483s) TCP 10.10.14.229:58053 > 10.129.2.47:139 S ttl=45 id=3188 iplen=44  seq=3049000324 win=1024 <mss 1460>
SENT (1.0494s) TCP 10.10.14.229:58055 > 10.129.2.47:139 S ttl=55 id=57738 iplen=44  seq=3049131398 win=1024 <mss 1460>
Nmap scan report for 10.129.2.47
Host is up.

PORT    STATE    SERVICE
139/tcp filtered netbios-ssn

Nmap done: 1 IP address (1 host up) scanned in 2.09 seconds
```
