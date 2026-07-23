# Firewall Evasion

## Determine Firewalls

### TCP SYN Scan

!!! abstract

    Hedef S ve A bayrağı gönderiyor.

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 21,22,25 -sS -Pn -n --disable-arp-ping --packet-trace
```

```output title="Output" hl_lines="6"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 19:34 +03
SENT (0.0824s) TCP 10.10.14.229:46845 > 10.129.2.47:21 S ttl=56 id=61325 iplen=44  seq=2895362369 win=1024 <mss 1460>
SENT (0.0824s) TCP 10.10.14.229:46845 > 10.129.2.47:25 S ttl=42 id=50511 iplen=44  seq=2895362369 win=1024 <mss 1460>
SENT (0.0825s) TCP 10.10.14.229:46845 > 10.129.2.47:22 S ttl=43 id=41944 iplen=44  seq=2895362369 win=1024 <mss 1460>
RCVD (0.2639s) TCP 10.129.2.47:21 > 10.10.14.229:46845 RA ttl=63 id=0 iplen=40  seq=0 win=0
RCVD (0.2643s) TCP 10.129.2.47:22 > 10.10.14.229:46845 SA ttl=63 id=0 iplen=44  seq=46394002 win=64240 <mss 1340>
SENT (1.8121s) TCP 10.10.14.229:46847 > 10.129.2.47:25 S ttl=57 id=54326 iplen=44  seq=2895231299 win=1024 <mss 1460>
Nmap scan report for 10.129.2.47
Host is up (0.18s latency).

PORT   STATE    SERVICE
21/tcp closed   ftp
22/tcp open     ssh
25/tcp filtered smtp

Nmap done: 1 IP address (1 host up) scanned in 2.60 seconds
```

### TCP ACK Scan

!!! abstract

    Hedef R bayrağı gönderiyor.

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 21,22,25 -sA -Pn -n --disable-arp-ping --packet-trace
```

```output title="Output" hl_lines="5"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 19:34 +03
SENT (0.0510s) TCP 10.10.14.229:49595 > 10.129.2.47:21 A ttl=41 id=10680 iplen=40  seq=0 win=1024
SENT (0.0510s) TCP 10.10.14.229:49595 > 10.129.2.47:22 A ttl=58 id=44925 iplen=40  seq=0 win=1024
SENT (0.0510s) TCP 10.10.14.229:49595 > 10.129.2.47:25 A ttl=46 id=26103 iplen=40  seq=0 win=1024
RCVD (0.2920s) TCP 10.129.2.47:22 > 10.10.14.229:49595 R ttl=63 id=0 iplen=40  seq=401272150 win=0
RCVD (0.2927s) TCP 10.129.2.47:21 > 10.10.14.229:49595 R ttl=63 id=0 iplen=40  seq=401272150 win=0
SENT (2.0192s) TCP 10.10.14.229:49597 > 10.129.2.47:25 A ttl=49 id=60262 iplen=40  seq=0 win=1024
Nmap scan report for 10.129.2.47
Host is up (0.24s latency).

PORT   STATE      SERVICE
21/tcp unfiltered ftp
22/tcp unfiltered ssh
25/tcp filtered   smtp

Nmap done: 1 IP address (1 host up) scanned in 3.03 seconds
```

## Decoys

!!! abstract

    Gerçek IP adresi rastgele üretilen IP adreslerinin arasına yerleştirilir.

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 80 -D RND:5 -Pn -n --disable-arp-ping --packet-trace
```

```output title="Output" hl_lines="6"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 19:41 +03
SENT (0.0415s) TCP 176.157.202.42:34876 > 10.129.2.47:80 S ttl=48 id=39339 iplen=44  seq=2730920685 win=1024 <mss 1460>
SENT (0.0415s) TCP 182.82.250.69:34876 > 10.129.2.47:80 S ttl=53 id=39339 iplen=44  seq=2730920685 win=1024 <mss 1460>
SENT (0.0415s) TCP 26.34.196.16:34876 > 10.129.2.47:80 S ttl=50 id=39339 iplen=44  seq=2730920685 win=1024 <mss 1460>
SENT (0.0416s) TCP 2.14.75.125:34876 > 10.129.2.47:80 S ttl=51 id=39339 iplen=44  seq=2730920685 win=1024 <mss 1460>
SENT (0.0416s) TCP 10.10.14.229:34876 > 10.129.2.47:80 S ttl=53 id=39339 iplen=44  seq=2730920685 win=1024 <mss 1460>
SENT (0.0416s) TCP 53.142.255.246:34876 > 10.129.2.47:80 S ttl=58 id=39339 iplen=44  seq=2730920685 win=1024 <mss 1460>
RCVD (0.2687s) TCP 10.129.2.47:80 > 10.10.14.229:34876 SA ttl=63 id=0 iplen=44  seq=2968985183 win=64240 <mss 1340>
Nmap scan report for 10.129.2.47
Host is up (0.23s latency).

PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 0.31 seconds
```

## DNS Proxying

### TCP SYN Scan of a Filtered Port

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 50000 -sS -Pn -n --disable-arp-ping --packet-trace
```

```output title="Output" hl_lines="8"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 19:48 +03
SENT (0.0726s) TCP 10.10.14.229:46602 > 10.129.2.47:50000 S ttl=52 id=6612 iplen=44  seq=1656714215 win=1024 <mss 1460>
SENT (1.0737s) TCP 10.10.14.229:46604 > 10.129.2.47:50000 S ttl=57 id=40352 iplen=44  seq=1656583141 win=1024 <mss 1460>
Nmap scan report for 10.129.2.47
Host is up.

PORT      STATE    SERVICE
50000/tcp filtered ibm-db2

Nmap done: 1 IP address (1 host up) scanned in 2.11 seconds
```

### TCP SYN Scan from DNS Port

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 50000 --source-port 53 -sS -Pn -n --disable-arp-ping --packet-trace
```

```output title="Output" hl_lines="10"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 19:48 +03
SENT (0.0562s) TCP 10.10.14.229:53 > 10.129.2.47:50000 S ttl=40 id=51706 iplen=44  seq=1747746033 win=1024 <mss 1460>
SENT (1.0573s) TCP 10.10.14.229:53 > 10.129.2.47:50000 S ttl=44 id=61962 iplen=44  seq=1747877107 win=1024 <mss 1460>
RCVD (1.2551s) TCP 10.129.2.47:50000 > 10.10.14.229:53 A ttl=63 id=0 iplen=40  seq=470875239 win=64240
RCVD (1.2552s) TCP 10.129.2.47:50000 > 10.10.14.229:53 SA ttl=63 id=0 iplen=44  seq=470875238 win=64240 <mss 1340>
Nmap scan report for 10.129.2.47
Host is up (1.2s latency).

PORT      STATE SERVICE
50000/tcp open  ibm-db2

Nmap done: 1 IP address (1 host up) scanned in 7.09 seconds
```

### Connect to the Filtered Port

```sh
my@attack:~$ sudo nc -vnp 53 10.129.2.47 50000
```

```output title="Output"
Connection to 10.129.2.47 50000 port [tcp/*] succeeded!
```
