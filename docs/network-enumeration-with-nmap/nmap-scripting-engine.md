# Nmap Scripting Engine

## Update the Script Database

```sh
my@attack:~$ sudo nmap --script-updatedb
```

## Finding Scripts

```sh
my@attack:~$ sudo find / -wholename "*nmap*scripts*" 2> /dev/null
```

## Default Scripts

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 22 -sC --script-trace
```

## Specifying Scripts

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 22 --script banner
```

```output title="Output" hl_lines="7"
Starting Nmap 7.95 ( https://nmap.org ) at 2024-11-12 18:32 +03
Nmap scan report for 10.129.2.47
Host is up (1.1s latency).

PORT   STATE SERVICE
22/tcp open  ssh
|_banner: SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.7

Nmap done: 1 IP address (1 host up) scanned in 2.12 seconds
```

## Vuln Category

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 80 --script vuln
```

## [Aggressive Scan](https://nmap.org/book/man-misc-options.html)

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p 80 -A
```
