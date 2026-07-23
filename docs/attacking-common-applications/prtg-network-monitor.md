# PRTG Network Monitor

## Default Credentials

* prtgadmin
* prtgadmin

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.136.234 -p- --open -sV
```

```output title="Output" hl_lines="15"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-08 20:05 +03
Nmap scan report for 10.129.136.234
Host is up (0.15s latency).
Not shown: 64528 closed tcp ports (reset), 989 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
8000/tcp  open  ssl/http      Splunkd httpd
8080/tcp  open  http          Indy httpd 18.1.37.13946 (Paessler PRTG bandwidth monitor)
8089/tcp  open  ssl/http      Splunkd httpd
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 480.53 seconds
```

## Version

```sh
my@attack:~$ curl -s 'http://10.129.136.234:8080/index.htm' -A 'Mozilla/5.0 (X11; Linux x86_64; rv:138.0) Gecko/20100101 Firefox/138.0' | grep -P 'PRTG Network Monitor [0-9]'
```

```html title="Output"
<span class="prtgversion">&nbsp;PRTG Network Monitor 18.1.37.13946 </span>
```
