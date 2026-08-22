# ACL Enumeration

## Rights of William Ley over Objects

### PowerView

```pwsh
PS C:\Users\htb-student> $SIDWilliam = ConvertTo-SID wley
PS C:\Users\htb-student> Get-DomainObjectAcl -Identity * -ResolveGUIDs | ? {$_.SecurityIdentifier -eq $SIDWilliam}
```

```output title="Output" hl_lines="2 4"
AceQualifier           : AccessAllowed
ObjectDN               : CN=Dana Amundsen,OU=DevOps,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights  : ExtendedRight
ObjectAceType          : User-Force-Change-Password
ObjectSID              : S-1-5-21-3842939050-3880317879-2865463114-1176
InheritanceFlags       : ContainerInherit
BinaryLength           : 56
AceType                : AccessAllowedObject
ObjectAceFlags         : ObjectAceTypePresent
IsCallback             : False
PropagationFlags       : None
SecurityIdentifier     : S-1-5-21-3842939050-3880317879-2865463114-1181
AccessMask             : 256
AuditFlags             : None
IsInherited            : False
AceFlags               : ContainerInherit
InheritedObjectAceType : All
OpaqueLength           : 0
```

### ActiveDirectory Module

#### Domain Users

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
```

#### Loop

```pwsh
PS C:\Users\htb-student> foreach ($user in [System.IO.File]::ReadLines("C:\Users\htb-student\ad_users.txt")) {Get-Acl "AD:$(ActiveDirectory\Get-ADUser $user)" | Select-Object Path -ExpandProperty Access | ? {$_.IdentityReference -match "INLANEFREIGHT\\wley"}}
```

```output title="Output" hl_lines="1 4"
Path                  : Microsoft.ActiveDirectory.Management.dll\ActiveDirectory:://RootDSE/CN=Dana Amundsen,OU=DevOps,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights : ExtendedRight
InheritanceType       : All
ObjectType            : 00299570-246d-11d0-a768-00aa006e0529
InheritedObjectType   : 00000000-0000-0000-0000-000000000000
ObjectFlags           : ObjectAceTypePresent
AccessControlType     : Allow
IdentityReference     : INLANEFREIGHT\wley
IsInherited           : False
InheritanceFlags      : ContainerInherit
PropagationFlags      : None
```

#### GUID to CN

```pwsh
PS C:\Users\htb-student> $GUID = "00299570-246d-11d0-a768-00aa006e0529"
PS C:\Users\htb-student> ActiveDirectory\Get-ADObject -SearchBase "CN=Extended-Rights,$((ActiveDirectory\Get-ADRootDSE).configurationNamingContext)" -Filter {ObjectClass -like "controlAccessRight"} -Properties * | ? {$_.rightsGuid -eq $GUID} | Select-Object CN
```

```output title="Output"
CN
--
User-Force-Change-Password
```

## Rights of Dana Amundsen over Objects

```pwsh
PS C:\Users\htb-student> $SIDDana = ConvertTo-SID damundsen
PS C:\Users\htb-student> Get-DomainObjectAcl -Identity * -ResolveGUIDs | ? {$_.SecurityIdentifier -eq $SIDDana}
```

```output title="Output" hl_lines="2 3"
AceType               : AccessAllowed
ObjectDN              : CN=Help Desk Level 1,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights : ListChildren, ReadProperty, GenericWrite
OpaqueLength          : 0
ObjectSID             : S-1-5-21-3842939050-3880317879-2865463114-4022
InheritanceFlags      : ContainerInherit
BinaryLength          : 36
IsInherited           : False
IsCallback            : False
PropagationFlags      : None
SecurityIdentifier    : S-1-5-21-3842939050-3880317879-2865463114-1176
AccessMask            : 131132
AuditFlags            : None
AceFlags              : ContainerInherit
AceQualifier          : AccessAllowed
```

## Group Enumeration (Help Desk Level 1)

```pwsh
PS C:\Users\htb-student> Get-DomainGroup -Identity "Help Desk Level 1"
```

```output title="Output" hl_lines="11"
usncreated            : 36950
grouptype             : GLOBAL_SCOPE, SECURITY
samaccounttype        : GROUP_OBJECT
samaccountname        : Help Desk Level 1
whenchanged           : 2/12/2025 6:42:55 AM
objectsid             : S-1-5-21-3842939050-3880317879-2865463114-4022
objectclass           : {top, group}
cn                    : Help Desk Level 1
usnchanged            : 3317913
dscorepropagationdata : {3/24/2022 3:52:58 PM, 3/24/2022 3:49:31 PM, 3/22/2022 3:03:17 AM, 3/22/2022 3:03:14 AM...}
memberof              : CN=Information Technology,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
distinguishedname     : CN=Help Desk Level 1,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
name                  : Help Desk Level 1
member                : {CN=Stella Blagg,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Marie Wright,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Jerrell Metzler,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Evelyn Mailloux,OU=Operations,OU=Logistics-HK,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL...}
whencreated           : 10/28/2021 1:09:07 AM
instancetype          : 4
objectguid            : e539dfde-c633-4a10-bb81-d7ee2d3deb8f
objectcategory        : CN=Group,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
```

## Rights of Information Technology over Objects

```pwsh
PS C:\Users\htb-student> $SIDInformation = ConvertTo-SID "Information Technology"
PS C:\Users\htb-student> Get-DomainObjectAcl -Identity * -ResolveGUIDs | ? {$_.SecurityIdentifier -eq $SIDInformation}
```

```output title="Output" hl_lines="2 3"
AceType               : AccessAllowed
ObjectDN              : CN=Angela Dunn,OU=Server Admin,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights : GenericAll
OpaqueLength          : 0
ObjectSID             : S-1-5-21-3842939050-3880317879-2865463114-1164
InheritanceFlags      : ContainerInherit
BinaryLength          : 36
IsInherited           : False
IsCallback            : False
PropagationFlags      : None
SecurityIdentifier    : S-1-5-21-3842939050-3880317879-2865463114-4016
AccessMask            : 983551
AuditFlags            : None
AceFlags              : ContainerInherit
AceQualifier          : AccessAllowed
```
