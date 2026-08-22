# Splunk

## Default Credentials

* admin
* changeme

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.136.234 -sV
```

```output title="Output" hl_lines="12 14"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-08 15:51 +03
Nmap scan report for 10.129.136.234
Host is up (0.38s latency).
Not shown: 991 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
8000/tcp open  ssl/http      Splunkd httpd
8080/tcp open  http          Indy httpd 18.1.37.13946 (Paessler PRTG bandwidth monitor)
8089/tcp open  ssl/http      Splunkd httpd
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 41.96 seconds
```
