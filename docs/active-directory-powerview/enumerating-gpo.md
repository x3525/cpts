# Enumerating GPO

## Gathering GPO Data

```pwsh
PS C:\Users\htb-student> Get-DomainGPO | Select-Object displayname
```

```output title="Output"
displayname
-----------
Default Domain Policy
Default Domain Controllers Policy
LAPS Install
LAPS
Disable LM Hash
Disable CMD.exe
Disallow removable media
Prevent software installs
Disable guest account
Disable SMBv1
Map home drive
Disable Forced Restarts
Screensaver
Applocker
Fine-grained password policy
Restrict Control Panel
User - MS Office
User - Browser Settings
Audit Policy
PowerShell logging
Disable Defender
```

## Rights of Domain Users over Objects

```pwsh
PS C:\Users\htb-student> $SID = ConvertTo-SID "Domain Users"
PS C:\Users\htb-student> Get-DomainGPO | Get-ObjectAcl | ? {$_.SecurityIdentifier -eq $SID}
```

```output title="Output" hl_lines="1 3"
ObjectDN              : CN={831DE3ED-40B1-4703-ABA7-8EA13B2EB118},CN=Policies,CN=System,DC=INLANEFREIGHT,DC=LOCAL
ObjectSID             :
ActiveDirectoryRights : CreateChild, DeleteChild, ReadProperty, WriteProperty, GenericExecute
BinaryLength          : 36
AceQualifier          : AccessAllowed
IsCallback            : False
OpaqueLength          : 0
AccessMask            : 131127
SecurityIdentifier    : S-1-5-21-2974783224-3764228556-2640795941-513
AceType               : AccessAllowed
AceFlags              : ContainerInherit
IsInherited           : False
InheritanceFlags      : ContainerInherit
PropagationFlags      : None
AuditFlags            : None
```

## GUID to DisplayName

```pwsh
PS C:\Users\htb-student> GroupPolicy\Get-GPO -Guid 831DE3ED-40B1-4703-ABA7-8EA13B2EB118
```

```output title="Output" hl_lines="1"
DisplayName      : Screensaver
DomainName       : INLANEFREIGHT.LOCAL
Owner            : INLANEFREIGHT\Domain Admins
Id               : 831de3ed-40b1-4703-aba7-8ea13b2eb118
GpoStatus        : AllSettingsEnabled
Description      :
CreationTime     : 8/26/2020 10:46:46 PM
ModificationTime : 8/26/2020 11:11:01 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 0, SysVol Version: 0
WmiFilter        :
```
