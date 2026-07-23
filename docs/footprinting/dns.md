# DNS

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 53 | Domain Name System (DNS) |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.63.137 -p 53 -sV -sC
```

## Dangerous Settings

| OPTION |
|---|
| allow-query |
| allow-recursion |
| allow-transfer |
| zone-statistics |

## Zone Files

| RECORD | DESCRIPTION |
|---|---|
| A | IPv4 adresi. |
| AAAA | IPv6 adresi. |
| CNAME | Takma ad. |
| MX | Posta sunucusu. |
| NS | Ad sunucusu. |
| PTR | Ters sorgu. |
| SOA | Yönetici bilgileri. |
| SRV | Bilgisayar adları ve port numaraları. |
| TXT | Metin notları. |

## DiG

### Server Version

```sh
my@attack:~$ dig CH TXT version.bind 10.129.120.85
```

```zone title="Output" hl_lines="13"
; <<>> DiG 9.20.4 <<>> CH TXT version.bind 10.129.120.85
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 55205
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;version.bind.                  CH      TXT

;; ANSWER SECTION:
version.bind.           0       CH      TXT     "dnsmasq-2.83"

;; Query time: 6 msec
;; SERVER: 192.168.1.1#53(192.168.1.1) (UDP)
;; WHEN: Mon Jan 06 20:01:37 +03 2025
;; MSG SIZE  rcvd: 66

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: REFUSED, id: 20124
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;10.129.120.85.                 CH      TXT

;; Query time: 4060 msec
;; SERVER: 192.168.1.1#53(192.168.1.1) (UDP)
;; WHEN: Mon Jan 06 20:01:41 +03 2025
;; MSG SIZE  rcvd: 31
```

### Reverse Lookup

```sh
my@attack:~$ dig -x 157.240.9.35
```

```zone title="Output" hl_lines="13"
; <<>> DiG 9.20.4 <<>> -x 157.240.9.35
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51743
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;35.9.240.157.in-addr.arpa.     IN      PTR

;; ANSWER SECTION:
35.9.240.157.in-addr.arpa. 829  IN      PTR     edge-star-mini-shv-01-sof1.facebook.com.

;; Query time: 6 msec
;; SERVER: 192.168.1.1#53(192.168.1.1) (UDP)
;; WHEN: Wed Jan 08 21:06:35 +03 2025
;; MSG SIZE  rcvd: 107
```

### NS Query

!!! abstract

    Domain adresini çözebilen diğer ad sunucularını bulmak için bilinen bir DNS sunucusu üzerinde ilgili domain için NS sorgusu gerçekleştirilebilir.

```sh
my@attack:~$ dig NS inlanefreight.htb @10.129.63.137
```

```zone title="Output" hl_lines="15"
; <<>> DiG 9.20.4 <<>> ns inlanefreight.htb @10.129.63.137
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 34036
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 2
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: b73f74ce632f683601000000677c08e6a2f51359148c9bb5 (good)
;; QUESTION SECTION:
;inlanefreight.htb.             IN      NS

;; ANSWER SECTION:
inlanefreight.htb.      604800  IN      NS      ns.inlanefreight.htb.

;; ADDITIONAL SECTION:
ns.inlanefreight.htb.   604800  IN      A       127.0.0.1

;; Query time: 220 msec
;; SERVER: 10.129.63.137#53(10.129.63.137) (UDP)
;; WHEN: Mon Jan 06 19:46:30 +03 2025
;; MSG SIZE  rcvd: 107
```

### ANY Query

```sh
my@attack:~$ dig ANY inlanefreight.htb @10.129.63.137
```

```zone title="Output" hl_lines="15-19"
; <<>> DiG 9.20.4 <<>> ANY inlanefreight.htb @10.129.63.137
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 5288
;; flags: qr aa rd; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 2
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 8983c88c42f35d6b01000000677c0cd499103db4b205daa8 (good)
;; QUESTION SECTION:
;inlanefreight.htb.             IN      ANY

;; ANSWER SECTION:
inlanefreight.htb.      604800  IN      TXT     "v=spf1 include:mailgun.org include:_spf.google.com include:spf.protection.outlook.com include:_spf.atlassian.net ip4:10.129.124.8 ip4:10.129.127.2 ip4:10.129.42.106 ~all"
inlanefreight.htb.      604800  IN      TXT     "MS=ms97310371"
inlanefreight.htb.      604800  IN      TXT     "atlassian-domain-verification=t1rKCy68JFszSdCKVpw64A1QksWdXuYFUeSXKU"
inlanefreight.htb.      604800  IN      SOA     inlanefreight.htb. root.inlanefreight.htb. 2 604800 86400 2419200 604800
inlanefreight.htb.      604800  IN      NS      ns.inlanefreight.htb.

;; ADDITIONAL SECTION:
ns.inlanefreight.htb.   604800  IN      A       127.0.0.1

;; Query time: 203 msec
;; SERVER: 10.129.63.137#53(10.129.63.137) (TCP)
;; WHEN: Mon Jan 06 20:03:16 +03 2025
;; MSG SIZE  rcvd: 437
```

### AXFR Query

!!! abstract

    Kayıtların bir sunucudan başka bir sunucuya aktarımı Asynchronous Full Transfer Zone olarak ifade edilir.

    AXFR sırasında TCP kullanılır.

| MESSAGE | DIRECTION |
|---|---|
| AXFR Request | Secondary Sever --> Primary Server |
| Start of Authority Record | Secondary Sever <-- Primary Server |
| DNS Record | Secondary Sever <-- Primary Server |
| Zone Transfer Complete | Secondary Sever <-- Primary Server |
| Acknowledgement | Secondary Sever --> Primary Server |

```sh
my@attack:~$ dig AXFR zonetransfer.me @nsztm1.digi.ninja
```

```zone title="Output"
; <<>> DiG 9.20.4 <<>> AXFR zonetransfer.me @nsztm1.digi.ninja
;; global options: +cmd
zonetransfer.me.        7200    IN      SOA     nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.        301     IN      TXT     "google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
zonetransfer.me.        7200    IN      MX      0 ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      10 ALT1.ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      10 ALT2.ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX2.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX3.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX4.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX5.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      A       5.196.105.14
zonetransfer.me.        7200    IN      NS      nsztm1.digi.ninja.
zonetransfer.me.        7200    IN      NS      nsztm2.digi.ninja.
zonetransfer.me.        300     IN      HINFO   "Casio fx-700G" "Windows XP"
_acme-challenge.zonetransfer.me. 301 IN TXT     "6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
_sip._tcp.zonetransfer.me. 14000 IN     SRV     0 0 5060 www.zonetransfer.me.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200 IN PTR www.zonetransfer.me.
asfdbauthdns.zonetransfer.me. 7900 IN   AFSDB   1 asfdbbox.zonetransfer.me.
asfdbbox.zonetransfer.me. 7200  IN      A       127.0.0.1
asfdbvolume.zonetransfer.me. 7800 IN    AFSDB   1 asfdbbox.zonetransfer.me.
canberra-office.zonetransfer.me. 7200 IN A      202.14.81.230
cmdexec.zonetransfer.me. 300    IN      TXT     "; ls"
contact.zonetransfer.me. 2592000 IN     TXT     "Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes"
dc-office.zonetransfer.me. 7200 IN      A       143.228.181.132
deadbeef.zonetransfer.me. 7201  IN      AAAA    dead:beaf::
dr.zonetransfer.me.     300     IN      LOC     53 20 56.558 N 1 38 33.526 W 0.00m 1m 10000m 10m
DZC.zonetransfer.me.    7200    IN      TXT     "AbCdEfG"
email.zonetransfer.me.  2222    IN      NAPTR   1 1 "P" "E2U+email" "" email.zonetransfer.me.zonetransfer.me.
email.zonetransfer.me.  7200    IN      A       74.125.206.26
Hello.zonetransfer.me.  7200    IN      TXT     "Hi to Josh and all his class"
home.zonetransfer.me.   7200    IN      A       127.0.0.1
Info.zonetransfer.me.   7200    IN      TXT     "ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information."
internal.zonetransfer.me. 300   IN      NS      intns1.zonetransfer.me.
internal.zonetransfer.me. 300   IN      NS      intns2.zonetransfer.me.
intns1.zonetransfer.me. 300     IN      A       81.4.108.41
intns2.zonetransfer.me. 300     IN      A       167.88.42.94
office.zonetransfer.me. 7200    IN      A       4.23.39.254
ipv6actnow.org.zonetransfer.me. 7200 IN AAAA    2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.    7200    IN      A       207.46.197.32
robinwood.zonetransfer.me. 302  IN      TXT     "Robin Wood"
rp.zonetransfer.me.     321     IN      RP      robin.zonetransfer.me. robinwood.zonetransfer.me.
sip.zonetransfer.me.    3333    IN      NAPTR   2 3 "P" "E2U+sip" "!^.*$!sip:customer-service@zonetransfer.me!" .
sqli.zonetransfer.me.   300     IN      TXT     "' or 1=1 --"
sshock.zonetransfer.me. 7200    IN      TXT     "() { :]}; echo ShellShocked"
staging.zonetransfer.me. 7200   IN      CNAME   www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301 IN A 127.0.0.1
testing.zonetransfer.me. 301    IN      CNAME   www.zonetransfer.me.
vpn.zonetransfer.me.    4000    IN      A       174.36.59.154
www.zonetransfer.me.    7200    IN      A       5.196.105.14
xss.zonetransfer.me.    300     IN      TXT     "'><script>alert('Boo')</script>"
zonetransfer.me.        7200    IN      SOA     nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
;; Query time: 53 msec
;; SERVER: 81.4.108.41#53(nsztm1.digi.ninja) (TCP)
;; WHEN: Wed Jan 08 22:37:47 +03 2025
;; XFR size: 50 records (messages 1, bytes 2085)
```

## [Fierce](https://github.com/mschwager/fierce)

```sh
my@attack:~$ fierce --domain zonetransfer.me
```

## Brute Forcing Subdomains

### Manual Method

```sh
my@attack:~$ while IFS= read -r sub; do dig "$sub".inlanefreight.htb @10.129.63.137 | grep "$sub" | grep -v ';\|SOA'; done < /usr/local/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
```

```zone title="Output"
ns.inlanefreight.htb.   604800  IN      A       127.0.0.1
mail1.inlanefreight.htb. 604800 IN      A       10.129.18.201
app.inlanefreight.htb.  604800  IN      A       10.129.18.15
```

### [DNSEnum](https://github.com/fwaeytens/dnsenum)

```sh
my@attack:~$ dnsenum inlanefreight.htb --dnsserver 10.129.63.137 -f /usr/local/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt --enum --pages 5 --scrap 5
```
