# Communication with Processes

## Active Connections

```pwsh
PS C:\Users\htb-student> netstat -ano
```

```output title="Output" hl_lines="22 46"
Active Connections

  Proto  Local Address          Foreign Address        State           PID
  TCP    0.0.0.0:21             0.0.0.0:0              LISTENING       2088
  TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       840
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:1433           0.0.0.0:0              LISTENING       3520
  TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING       972
  TCP    0.0.0.0:5985           0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:8080           0.0.0.0:0              LISTENING       2312
  TCP    0.0.0.0:47001          0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:49664          0.0.0.0:0              LISTENING       544
  TCP    0.0.0.0:49665          0.0.0.0:0              LISTENING       104
  TCP    0.0.0.0:49666          0.0.0.0:0              LISTENING       964
  TCP    0.0.0.0:49667          0.0.0.0:0              LISTENING       1944
  TCP    0.0.0.0:49668          0.0.0.0:0              LISTENING       1800
  TCP    0.0.0.0:49669          0.0.0.0:0              LISTENING       688
  TCP    0.0.0.0:49670          0.0.0.0:0              LISTENING       680
  TCP    10.129.58.16:139       0.0.0.0:0              LISTENING       4
  TCP    10.129.58.16:3389      10.10.14.164:50484     ESTABLISHED     972
  TCP    127.0.0.1:14147        0.0.0.0:0              LISTENING       2088
  TCP    127.0.0.1:49671        127.0.0.1:49672        ESTABLISHED     2312
  TCP    127.0.0.1:49672        127.0.0.1:49671        ESTABLISHED     2312
  TCP    127.0.0.1:49673        127.0.0.1:49674        ESTABLISHED     2312
  TCP    127.0.0.1:49674        127.0.0.1:49673        ESTABLISHED     2312
  TCP    127.0.0.1:49675        127.0.0.1:49676        ESTABLISHED     2312
  TCP    127.0.0.1:49676        127.0.0.1:49675        ESTABLISHED     2312
  TCP    172.16.20.45:139       0.0.0.0:0              LISTENING       4
  TCP    [::]:21                [::]:0                 LISTENING       2088
  TCP    [::]:80                [::]:0                 LISTENING       4
  TCP    [::]:135               [::]:0                 LISTENING       840
  TCP    [::]:445               [::]:0                 LISTENING       4
  TCP    [::]:1433              [::]:0                 LISTENING       3520
  TCP    [::]:3389              [::]:0                 LISTENING       972
  TCP    [::]:5985              [::]:0                 LISTENING       4
  TCP    [::]:8080              [::]:0                 LISTENING       2312
  TCP    [::]:47001             [::]:0                 LISTENING       4
  TCP    [::]:49664             [::]:0                 LISTENING       544
  TCP    [::]:49665             [::]:0                 LISTENING       104
  TCP    [::]:49666             [::]:0                 LISTENING       964
  TCP    [::]:49667             [::]:0                 LISTENING       1944
  TCP    [::]:49668             [::]:0                 LISTENING       1800
  TCP    [::]:49669             [::]:0                 LISTENING       688
  TCP    [::]:49670             [::]:0                 LISTENING       680
  TCP    [::1]:14147            [::]:0                 LISTENING       2088
  UDP    0.0.0.0:123            *:*                                    1052
  UDP    0.0.0.0:500            *:*                                    964
  UDP    0.0.0.0:3389           *:*                                    972
  UDP    0.0.0.0:4500           *:*                                    964
  UDP    0.0.0.0:5050           *:*                                    1052
  UDP    0.0.0.0:5353           *:*                                    1156
  UDP    0.0.0.0:5355           *:*                                    1156
  UDP    10.129.58.16:137       *:*                                    4
  UDP    10.129.58.16:138       *:*                                    4
  UDP    10.129.58.16:1900      *:*                                    3668
  UDP    10.129.58.16:65089     *:*                                    3668
  UDP    127.0.0.1:1900         *:*                                    3668
  UDP    127.0.0.1:65090        *:*                                    3668
  UDP    172.16.20.45:137       *:*                                    4
  UDP    172.16.20.45:138       *:*                                    4
  UDP    172.16.20.45:1900      *:*                                    3668
  UDP    172.16.20.45:65088     *:*                                    3668
  UDP    [::]:123               *:*                                    1052
  UDP    [::]:500               *:*                                    964
  UDP    [::]:3389              *:*                                    972
  UDP    [::]:4500              *:*                                    964
  UDP    [::]:5353              *:*                                    1156
  UDP    [::]:5355              *:*                                    1156
  UDP    [::1]:1900             *:*                                    3668
  UDP    [::1]:65087            *:*                                    3668
  UDP    [fe80::5196:ca37:70a7:5de9%2]:1900  *:*                                    3668
  UDP    [fe80::5196:ca37:70a7:5de9%2]:65085  *:*                                    3668
  UDP    [fe80::add8:f5c1:76a0:4106%4]:1900  *:*                                    3668
  UDP    [fe80::add8:f5c1:76a0:4106%4]:65086  *:*                                    3668
```

## Named Pipes

### [PipeList](https://learn.microsoft.com/en-us/sysinternals/downloads/pipelist)

```pwsh
PS C:\Users\htb-student> C:\Tools\PipeList\pipelist.exe /accepteula
```

```output title="Output"
PipeList v1.02 - Lists open named pipes
Copyright (C) 2005-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

Pipe Name                                    Instances       Max Instances
---------                                    ---------       -------------
InitShutdown                                      3               -1
lsass                                             4               -1
ntsvcs                                            3               -1
scerpc                                            3               -1
Winsock2\CatalogChangeListener-348-0              1                1
epmapper                                          3               -1
Winsock2\CatalogChangeListener-220-0              1                1
LSM_API_service                                   3               -1
eventlog                                          3               -1
Winsock2\CatalogChangeListener-68-0               1                1
TermSrv_API_service                               3               -1
Ctx_WinStation_API_service                        3               -1
wkssvc                                            4               -1
SessEnvPublicRpc                                  3               -1
Winsock2\CatalogChangeListener-3c4-0              1                1
atsvc                                             3               -1
trkwks                                            3               -1
W32TIME_ALT                                       3               -1
spoolss                                           3               -1
Winsock2\CatalogChangeListener-798-0              1                1
WindscribeService                                 1               -1
srvsvc                                            6               -1
vgauth-service                                    1               -1
Winsock2\CatalogChangeListener-708-0              1                1
Winsock2\CatalogChangeListener-2b0-0              1                1
Winsock2\CatalogChangeListener-2a8-0              1                1
SQLLocal\SQLEXPRESS01                             3               -1
MSSQL$SQLEXPRESS01\sql\query                      2               -1
PIPE_EVENTROOT\CIMV2SCM EVENT PROVIDER            1               -1
TDLN-3700-41                                      1                2
TDLN-6168-41                                      1                2
TDLN-760-41                                       1                2
TDLN-3728-41                                      1                2
```

### PowerShell

```pwsh
PS C:\Users\htb-student> Get-ChildItem \\.\pipe\
```

```output title="Output"
    Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              3 InitShutdown
------       12/31/1600   4:00 PM              4 lsass
------       12/31/1600   4:00 PM              3 ntsvcs
------       12/31/1600   4:00 PM              3 scerpc


    Directory: \\.\pipe\Winsock2


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-348-0


    Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              3 epmapper


    Directory: \\.\pipe\Winsock2


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-220-0


    Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              3 LSM_API_service
------       12/31/1600   4:00 PM              3 eventlog


    Directory: \\.\pipe\Winsock2


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-68-0


    Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              3 TermSrv_API_service
------       12/31/1600   4:00 PM              3 Ctx_WinStation_API_service
------       12/31/1600   4:00 PM              4 wkssvc
------       12/31/1600   4:00 PM              3 SessEnvPublicRpc


    Directory: \\.\pipe\Winsock2


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-3c4-0


    Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              3 atsvc
------       12/31/1600   4:00 PM              3 trkwks
------       12/31/1600   4:00 PM              3 W32TIME_ALT
------       12/31/1600   4:00 PM              3 spoolss


    Directory: \\.\pipe\Winsock2


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-798-0


    Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 WindscribeService
------       12/31/1600   4:00 PM              6 srvsvc
------       12/31/1600   4:00 PM              1 vgauth-service


    Directory: \\.\pipe\Winsock2


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-708-0
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-2b0-0
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-2a8-0


    Directory: \\.\pipe\SQLLocal


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              3 SQLLocal\SQLEXPRESS01


    Directory: \\.\pipe\MSSQL$SQLEXPRESS01\sql


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              2 MSSQL$SQLEXPRESS01\sql\query


    Directory: \\.\pipe\PIPE_EVENTROOT


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 PIPE_EVENTROOT\CIMV2SCM EVENT PROVIDER


    Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 TDLN-7916-41
------       12/31/1600   4:00 PM              1 TDLN-4644-41
------       12/31/1600   4:00 PM              1 TDLN-5192-41
------       12/31/1600   4:00 PM              1 TDLN-1040-41
```

## [AccessChk](https://learn.microsoft.com/en-us/sysinternals/downloads/accesschk) (Searching for Write Access)

```pwsh
PS C:\Users\htb-student> C:\Tools\AccessChk\accesschk.exe /accepteula \pipe\* -w -v
```

## AccessChk

```pwsh
PS C:\Users\htb-student> C:\Tools\AccessChk\accesschk.exe /accepteula \pipe\WindscribeService -v
```

```output title="Output" hl_lines="8"
Accesschk v6.13 - Reports effective permissions for securable objects
Copyright ⌐ 2006-2020 Mark Russinovich
Sysinternals - www.sysinternals.com

\\.\Pipe\WindscribeService
  Medium Mandatory Level (Default) [No-Write-Up]
  RW Everyone
        FILE_ALL_ACCESS
```
