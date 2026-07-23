# Pivoting Around Obstacles

## [plink](https://the.earth.li/~sgtatham/putty/0.83/htmldoc/Chapter7.html#plink)

### Forwarding

```pwsh
PS C:\Users\my> plink -ssh -D localhost:9050 ubuntu@10.129.15.50
```

### [Proxifier](https://www.proxifier.com/) Profile Creation

1. Profile
2. Proxy Servers
3. Add
4. Server
    * Address: 127.0.0.1
    * Port: 9050
    * Protocol: SOCKS Version 5

### RDP Connection

```pwsh
PS C:\Users\my> C:\Windows\System32\mstsc.exe
```

1. Show Options
2. Logon settings
    * Computer: 172.16.5.19
    * User name: victor
    * Password: pass@123

## [sshuttle](https://github.com/sshuttle/sshuttle)

### Starting the Proxy

```sh
my@attack:~$ sudo sshuttle -r ubuntu:@10.129.15.50 172.16.5.0/23 -v
```

```output title="Output" hl_lines="33-39"
Starting sshuttle proxy (version 1.2.0).
c : Starting firewall manager with command: ['/usr/bin/sshuttle', '-v', '--method', 'auto', '--firewall']
fw: Starting firewall with Python version 3.13.3
fw: ready method name nat.
c : Using default IPv4 listen address 127.0.0.1
c : IPv6 enabled: Using default IPv6 listen address ::1
c : Method: nat
c : IPv4: on
c : IPv6: on
c : UDP : off (not available with nat method)
c : DNS : off (available)
c : User: off (available)
c : Subnets to forward through remote host (type, IP, cidr mask width, startPort, endPort):
c :   (<AddressFamily.AF_INET: 2>, '172.16.5.0', 23, 0, 0)
c : Subnets to exclude from forwarding:
c :   (<AddressFamily.AF_INET: 2>, '127.0.0.1', 32, 0, 0)
c :   (<AddressFamily.AF_INET6: 10>, '::1', 128, 0, 0)
c : TCP redirector listening on ('::1', 12300, 0, 0).
c : TCP redirector listening on ('127.0.0.1', 12300).
c : Starting client with Python version 3.13.3
c : Connecting to server...
 s: Running server on remote host with /usr/bin/python3 (version 3.8.10)
 s: latency control setting = True
 s: auto-nets:False
c : Connected to server.
fw: setting up.
fw: ip6tables -w -t nat -N sshuttle-12300
fw: ip6tables -w -t nat -F sshuttle-12300
fw: ip6tables -w -t nat -I OUTPUT 1 -j sshuttle-12300
fw: ip6tables -w -t nat -I PREROUTING 1 -j sshuttle-12300
fw: ip6tables -w -t nat -A sshuttle-12300 -j RETURN --dest ::1/128 -p tcp
fw: ip6tables -w -t nat -A sshuttle-12300 -j RETURN -m addrtype --dst-type LOCAL
fw: iptables -w -t nat -N sshuttle-12300
fw: iptables -w -t nat -F sshuttle-12300
fw: iptables -w -t nat -I OUTPUT 1 -j sshuttle-12300
fw: iptables -w -t nat -I PREROUTING 1 -j sshuttle-12300
fw: iptables -w -t nat -A sshuttle-12300 -j RETURN --dest 127.0.0.1/32 -p tcp
fw: iptables -w -t nat -A sshuttle-12300 -j REDIRECT --dest 172.16.5.0/23 -p tcp --to-ports 12300
fw: iptables -w -t nat -A sshuttle-12300 -j RETURN -m addrtype --dst-type LOCAL
```

### Test Scan

```sh
my@attack:~$ nmap 172.16.5.19 -p 3389 -sV -Pn
```

```output title="Output" hl_lines="6"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-02 17:56 +03
Nmap scan report for 172.16.5.19
Host is up (0.00028s latency).

PORT     STATE SERVICE       VERSION
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.43 seconds
```

## [rpivot](https://github.com/klsecservices/rpivot)

### Server on Attack Machine

```sh
my@attack:~/rpivot$ python2 server.py --server-ip 0.0.0.0 --server-port 9999 --proxy-port 9050
```

### Client on Pivot

```sh
ubuntu@WEB01:~/rpivot$ python2 client.py --server-ip 10.10.15.109 --server-port 9999
```

### Firefox

```sh
my@attack:~$ proxychains -q firefox "http://172.16.5.135:80"
```

## [netsh](https://learn.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts)

### Port Forward

```pwsh
PS C:\Users\htb-student> netsh interface portproxy add v4tov4 listenaddress=10.129.15.50 listenport=8080 connectaddress=172.16.5.19 connectport=3389
```

### Verifying

```pwsh
PS C:\Users\htb-student> netsh interface portproxy show v4tov4
```

```output title="Output"
Listen on ipv4:             Connect to ipv4:

Address         Port        Address         Port
--------------- ----------  --------------- ----------
10.129.15.50    8080        172.16.5.19     3389
```

### xFreeRDP

```sh
my@attack:~$ xfreerdp /v:10.129.15.50:8080 /u:'victor' /p:'pass@123'
```
