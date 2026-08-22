# Initial Enumeration

## Running Processes

```pwsh
PS C:\Users\htb-student> tasklist /svc
```

```output title="Output" hl_lines="5 11 40"
Image Name                     PID Services
========================= ======== ============================================
System Idle Process              0 N/A
System                           4 N/A
smss.exe                       332 N/A
csrss.exe                      440 N/A
wininit.exe                    544 N/A
csrss.exe                      552 N/A
winlogon.exe                   608 N/A
services.exe                   680 N/A
lsass.exe                      688 KeyIso, SamSs, VaultSvc
svchost.exe                    780 BrokerInfrastructure, DcomLaunch, LSM,
                                   PlugPlay, Power, SystemEventsBroker
svchost.exe                    840 RpcEptMapper, RpcSs
dwm.exe                        936 N/A
svchost.exe                    964 Appinfo, CertPropSvc, DsmSvc, gpsvc,
                                   IKEEXT, iphlpsvc, lfsvc, ProfSvc, Schedule,
                                   SENS, SessionEnv, ShellHWDetection, Themes,
                                   UserManager, Winmgmt, WpnService, wuauserv
svchost.exe                    972 TermService
svchost.exe                    104 Dhcp, EventLog, lmhosts, TimeBrokerSvc
svchost.exe                    368 NcbService, PcaSvc, ScDeviceEnum, StorSvc,
                                   TrkWks, UALSVC, UmRdpService,
                                   WdiSystemHost, wudfsvc
svchost.exe                   1052 EventSystem, FontCache, LicenseManager,
                                   netprofm, nsi, W32Time, WinHttpAutoProxySvc
vm3dservice.exe               1076 vm3dservice
svchost.exe                   1124 BFE, CoreMessagingRegistrar, DPS, MpsSvc
svchost.exe                   1156 CryptSvc, Dnscache, LanmanWorkstation,
                                   NlaSvc, WinRM
svchost.exe                   1364 Wcmsvc
svchost.exe                   1452 WlanSvc
svchost.exe                   1800 PolicyAgent
spoolsv.exe                   1944 Spooler
svchost.exe                   1332 AppHostSvc
svchost.exe                   1552 DiagTrack
svchost.exe                   1376 ftpsvc
inetinfo.exe                  1796 IISADMIN
svchost.exe                   2064 StateRepository, tiledatamodelsvc
FileZilla Server.exe          2088 FileZilla Server
sqlwriter.exe                 2096 SQLWriter
svchost.exe                   2112 W3SVC, WAS
vmtoolsd.exe                  2140 VMTools
MsMpEng.exe                   2152 WinDefend
VGAuthService.exe             2196 VGAuthService
svchost.exe                   2272 LanmanServer
WindscribeService.exe         2296 WindscribeService
Tomcat8.exe                   2312 Tomcat8
conhost.exe                   2516 N/A
dllhost.exe                   3384 COMSysApp
sqlservr.exe                  3520 MSSQL$SQLEXPRESS01
sqlceip.exe                   3536 SQLTELEMETRY$SQLEXPRESS01
msdtc.exe                     3688 MSDTC
WmiPrvSE.exe                  3712 N/A
WmiPrvSE.exe                  4744 N/A
RuntimeBroker.exe             5008 N/A
sihost.exe                    5112 N/A
svchost.exe                   4340 CDPUserSvc_4d040, OneSyncSvc_4d040
taskhostw.exe                 4448 N/A
GoogleUpdate.exe              4564 N/A
ServerManager.exe             5816 N/A
svchost.exe                   3668 SSDPSRV
vm3dservice.exe               5536 N/A
vmtoolsd.exe                  5956 N/A
FileZilla Server Interfac     3396 N/A
jusched.exe                   2912 N/A
csrss.exe                     5208 N/A
winlogon.exe                  5232 N/A
dwm.exe                       3864 N/A
rdpclip.exe                   6300 N/A
sihost.exe                    6396 N/A
svchost.exe                   6404 CDPUserSvc_7b9f0, OneSyncSvc_7b9f0
taskhostw.exe                 6444 N/A
RuntimeBroker.exe             6568 N/A
vm3dservice.exe               7320 N/A
jusched.exe                   7464 N/A
powershell.exe                7720 N/A
conhost.exe                   7732 N/A
taskhostw.exe                 1880 N/A
explorer.exe                  1576 N/A
explorer.exe                  5844 N/A
ShellExperienceHost.exe       8140 N/A
ShellExperienceHost.exe       4604 N/A
SearchUI.exe                  3036 N/A
SearchUI.exe                  5156 N/A
tasklist.exe                  6084 N/A
```

## Environment Variables

```pwsh
PS C:\Users\htb-student> cmd.exe /c set
```

```output title="Output" hl_lines="9 16"
ALLUSERSPROFILE=C:\ProgramData
APPDATA=C:\Users\htb-student\AppData\Roaming
CLIENTNAME=linux
CommonProgramFiles=C:\Program Files\Common Files
CommonProgramFiles(x86)=C:\Program Files (x86)\Common Files
CommonProgramW6432=C:\Program Files\Common Files
COMPUTERNAME=WINLPE-SRV01
ComSpec=C:\Windows\system32\cmd.exe
HOMEDRIVE=C:
HOMEPATH=\Users\htb-student
JAVA_HOME=C:\Program Files\Java\jre1.8.0_231
LOCALAPPDATA=C:\Users\htb-student\AppData\Local
LOGONSERVER=\\WINLPE-SRV01
NUMBER_OF_PROCESSORS=6
OS=Windows_NT
Path=C:\Program Files (x86)\Common Files\Oracle\Java\javapath;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\130\Tools\Binn\;C:\Program Files (x86)\Microsoft SQL Server\130\Tools\Binn\;C:\Program Files\Microsoft SQL Server\130\Tools\Binn\;C:\Program Files\Microsoft SQL Server\130\DTS\Binn\;C:\Program Files (x86)\Microsoft SQL Server\150\DTS\Binn\;C:\Program Files\Azure Data Studio\bin;C:\Program Files\PuTTY\;C:\Users\htb-student\AppData\Local\Microsoft\WindowsApps;
PATHEXT=.COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC;.CPL
PROCESSOR_ARCHITECTURE=AMD64
PROCESSOR_IDENTIFIER=AMD64 Family 25 Model 1 Stepping 1, AuthenticAMD
PROCESSOR_LEVEL=25
PROCESSOR_REVISION=0101
ProgramData=C:\ProgramData
ProgramFiles=C:\Program Files
ProgramFiles(x86)=C:\Program Files (x86)
ProgramW6432=C:\Program Files
PROMPT=$P$G
PSModulePath=C:\Users\htb-student\Documents\WindowsPowerShell\Modules;C:\Program Files\WindowsPowerShell\Modules;C:\Windows\system32\WindowsPowerShell\v1.0\Modules;C:\Program Files (x86)\Microsoft SQL Server\130\Tools\PowerShell\Modules\
PUBLIC=C:\Users\Public
SESSIONNAME=RDP-Tcp#0
SystemDrive=C:
SystemRoot=C:\Windows
TEMP=C:\Users\HTB-ST~1\AppData\Local\Temp\2
TMP=C:\Users\HTB-ST~1\AppData\Local\Temp\2
USERDOMAIN=WINLPE-SRV01
USERDOMAIN_ROAMINGPROFILE=WINLPE-SRV01
USERNAME=htb-student
USERPROFILE=C:\Users\htb-student
windir=C:\Windows
```

## System Information

```pwsh
PS C:\Users\htb-student> systeminfo
```

```output title="Output" hl_lines="3 12-13 32 34-38"
Host Name:                 WINLPE-SRV01
OS Name:                   Microsoft Windows Server 2016 Standard
OS Version:                10.0.14393 N/A Build 14393
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:
Product ID:                00376-30821-30176-AA890
Original Install Date:     3/24/2021, 3:46:32 PM
System Boot Time:          6/17/2025, 3:53:22 PM
System Manufacturer:       VMware, Inc.
System Model:              VMware7,1
System Type:               x64-based PC
Processor(s):              3 Processor(s) Installed.
                           [01]: AMD64 Family 25 Model 1 Stepping 1 AuthenticAMD ~2445 Mhz
                           [02]: AMD64 Family 25 Model 1 Stepping 1 AuthenticAMD ~2445 Mhz
                           [03]: AMD64 Family 25 Model 1 Stepping 1 AuthenticAMD ~2445 Mhz
BIOS Version:              VMware, Inc. VMW71.00V.24224532.B64.2408191458, 8/19/2024
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume2
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC-08:00) Pacific Time (US & Canada)
Total Physical Memory:     6,143 MB
Available Physical Memory: 4,056 MB
Virtual Memory: Max Size:  7,167 MB
Virtual Memory: Available: 4,838 MB
Virtual Memory: In Use:    2,329 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              \\WINLPE-SRV01
Hotfix(s):                 4 Hotfix(s) Installed.
                           [01]: KB3199986
                           [02]: KB4054590
                           [03]: KB5001078
                           [04]: KB3200970
Network Card(s):           3 NIC(s) Installed.
                           [01]: vmxnet3 Ethernet Adapter
                                 Connection Name: Ethernet0 2
                                 DHCP Enabled:    Yes
                                 DHCP Server:     10.129.0.1
                                 IP address(es)
                                 [01]: 10.129.58.16
                                 [02]: fe80::add8:f5c1:76a0:4106
                                 [03]: dead:beef::add8:f5c1:76a0:4106
                                 [04]: dead:beef::111
                           [02]: vmxnet3 Ethernet Adapter
                                 Connection Name: Ethernet1
                                 DHCP Enabled:    No
                                 IP address(es)
                                 [01]: 172.16.20.45
                                 [02]: fe80::5196:ca37:70a7:5de9
                           [03]: Windscribe VPN
                                 Connection Name: Ethernet
                                 Status:          Media disconnected
Hyper-V Requirements:      A hypervisor has been detected. Features required for Hyper-V will not be displayed.
```

## Patches

```pwsh
PS C:\Users\htb-student> wmic QFE
```

```output title="Output"
Caption                                     CSName        Description      FixComments  HotFixID   InstallDate  InstalledBy                 InstalledOn  Name  ServicePackInEffect  Status
http://support.microsoft.com/?kbid=3199986  WINLPE-SRV01  Update                        KB3199986               NT AUTHORITY\SYSTEM         11/21/2016
http://support.microsoft.com/?kbid=4054590  WINLPE-SRV01  Update                        KB4054590               WINLPE-SRV01\Administrator  3/30/2021
https://support.microsoft.com/help/5001078  WINLPE-SRV01  Security Update               KB5001078               NT AUTHORITY\SYSTEM         3/25/2021
http://support.microsoft.com/?kbid=3200970  WINLPE-SRV01  Security Update               KB3200970               WINLPE-SRV01\Administrator  4/13/2021
```

## Patches (PowerShell)

```pwsh
PS C:\Users\htb-student> Get-HotFix | ft -AutoSize
```

```output title="Output"
Source       Description     HotFixID  InstalledBy                InstalledOn
------       -----------     --------  -----------                -----------
WINLPE-SRV01 Update          KB3199986 NT AUTHORITY\SYSTEM        11/21/2016 12:00:00 AM
WINLPE-SRV01 Update          KB4054590 WINLPE-SRV01\Administrator 3/30/2021 12:00:00 AM
WINLPE-SRV01 Security Update KB5001078 NT AUTHORITY\SYSTEM        3/25/2021 12:00:00 AM
WINLPE-SRV01 Security Update KB3200970 WINLPE-SRV01\Administrator 4/13/2021 12:00:00 AM
```

## Installed Programs

```pwsh
PS C:\Users\htb-student> Get-WmiObject -Class Win32_Product
```

## Installed Programs (Registry)

```pwsh
PS C:\Users\htb-student> $INSTALLED = Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" |  Select-Object DisplayName,DisplayVersion,InstallLocation
PS C:\Users\htb-student> $INSTALLED += Get-ItemProperty -Path "HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | Select-Object DisplayName,DisplayVersion,InstallLocation
PS C:\Users\htb-student> $INSTALLED | ? {$_.DisplayName -ne $null} | Sort-Object -Property DisplayName -Unique | Format-Table -AutoSize
```

## Current User

```pwsh
PS C:\Users\htb-student> whoami /user
```

```output title="Output"
USER INFORMATION
----------------

User Name                SID
======================== ==============================================
winlpe-srv01\htb-student S-1-5-21-3769161915-3336846931-3985975925-1004
```

## Current User Privileges

```pwsh
PS C:\Users\htb-student> whoami /priv
```

```output title="Output"
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

## Current User Groups

```pwsh
PS C:\Users\htb-student> whoami /groups
```

```output title="Output"
GROUP INFORMATION
-----------------

Group Name                             Type             SID          Attributes
====================================== ================ ============ ==================================================
Everyone                               Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Desktop Users           Alias            S-1-5-32-555 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                          Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\REMOTE INTERACTIVE LOGON  Well-known group S-1-5-14     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE               Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users       Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization         Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account             Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
LOCAL                                  Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication       Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level Label            S-1-16-8192
```

## All Users

```pwsh
PS C:\Users\htb-student> net user
```

```output title="Output"
User accounts for \\WINLPE-SRV01

-------------------------------------------------------------------------------
Administrator            DefaultAccount           Guest
helpdesk                 htb-student              htb-student_adm
jordan                   logger                   mrb3n
sarah                    sccm_svc                 secsvc
sql_dev
```

## All Groups

```pwsh
PS C:\Users\htb-student> net localgroup
```

```output title="Output"
Aliases for \\WINLPE-SRV01

--------------------------------------------
*Access Control Assistance Operators
*Administrators
*Backup Operators
*Certificate Service DCOM Access
*Cryptographic Operators
*Distributed COM Users
*Event Log Readers
*Guests
*Hyper-V Administrators
*IIS_IUSRS
*Network Configuration Operators
*Performance Log Users
*Performance Monitor Users
*Power Users
*Print Operators
*RDS Endpoint Servers
*RDS Management Servers
*RDS Remote Access Servers
*Remote Desktop Users
*Remote Management Users
*Replicator
*SQLServer2005SQLBrowserUser$WINLPE-SRV01
*Storage Replica Administrators
*System Managed Accounts Group
*Users
The command completed successfully.
```

## Details About a Group

```pwsh
PS C:\Users\htb-student> net localgroup Administrators
```

```output title="Output"
Aliases for \\WINLPE-SRV01

Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
helpdesk
htb-student_adm
mrb3n
sccm_svc
secsvc
The command completed successfully.
```

## Password Policy

```pwsh
PS C:\Users\htb-student> net accounts
```

```output title="Output" hl_lines="4"
Force user logoff how long after time expires?:       Never
Minimum password age (days):                          0
Maximum password age (days):                          Unlimited
Minimum password length:                              0
Length of password history maintained:                None
Lockout threshold:                                    Never
Lockout duration (minutes):                           30
Lockout observation window (minutes):                 30
Computer role:                                        SERVER
The command completed successfully.
```
