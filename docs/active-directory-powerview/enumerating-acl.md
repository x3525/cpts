# Enumerating ACL

## Rights of Objects over Joe Evans

```pwsh
PS C:\Users\htb-student> (Get-Acl "AD:$((ActiveDirectory\Get-ADUser joe.evans).distinguishedname)").access | ? {$_.ActiveDirectoryRights -match "WriteProperty" -or $_.ActiveDirectoryRights -match "GenericAll"} | Select-Object IdentityReference,ActiveDirectoryRights -Unique | ft -Wrap
```

```output title="Output" hl_lines="6"
IdentityReference                                                                                                ActiveDirectoryRights
-----------------                                                                                                ---------------------
NT AUTHORITY\SYSTEM                                                                                                         GenericAll
S-1-5-32-548                                                                                                                GenericAll
INLANEFREIGHT\Domain Admins                                                                                                 GenericAll
INLANEFREIGHT\douglas.bull                                                                                                  GenericAll
NT AUTHORITY\SELF                                                                                          ReadProperty, WriteProperty
S-1-5-32-561                                                                                               ReadProperty, WriteProperty
INLANEFREIGHT\Cert Publishers                                                                              ReadProperty, WriteProperty
INLANEFREIGHT\Organization Management                                                                                    WriteProperty
INLANEFREIGHT\Exchange Trusted Subsystem                                                                                 WriteProperty
INLANEFREIGHT\Exchange Windows Permissions                                                                               WriteProperty
INLANEFREIGHT\Exchange Servers                                                                                           WriteProperty
INLANEFREIGHT\Key Admins                                                                                   ReadProperty, WriteProperty
INLANEFREIGHT\Enterprise Key Admins                                                                        ReadProperty, WriteProperty
INLANEFREIGHT\Exchange Trusted Subsystem               CreateChild, DeleteChild, ListChildren, ReadProperty, WriteProperty, ListObject
INLANEFREIGHT\Exchange Servers                         CreateChild, DeleteChild, ListChildren, ReadProperty, WriteProperty, ListObject
INLANEFREIGHT\Organization Management                                                                                       GenericAll
INLANEFREIGHT\Exchange Trusted Subsystem                                                                                    GenericAll
NT AUTHORITY\SELF                                                                                                        WriteProperty
NT AUTHORITY\SELF                                                                           ReadProperty, WriteProperty, ExtendedRight
INLANEFREIGHT\Enterprise Admins                                                                                             GenericAll
BUILTIN\Administrators                     CreateChild, Self, WriteProperty, ExtendedRight, Delete, GenericRead, WriteDacl, WriteOwner
NT AUTHORITY\ANONYMOUS LOGON                                                               ReadProperty, WriteProperty, GenericExecute
```

## Find-InterestingDomainAcl

```pwsh
PS C:\Users\htb-student> Find-InterestingDomainAcl -Domain INLANEFREIGHT.LOCAL -ResolveGUIDs
```

## Get-DomainObjectAcl

```pwsh
PS C:\Users\htb-student> Get-DomainObjectAcl -Domain INLANEFREIGHT.LOCAL -Identity harry.jones -ResolveGUIDs
```

## Network Shares

```pwsh
PS C:\Users\htb-student> Get-NetShare -ComputerName WS01
```

```output title="Output"
Name                  Type Remark        ComputerName
----                  ---- ------        ------------
ADMIN$          2147483648 Remote Admin  WS01
C$              2147483648 Default share WS01
Client_Invoices          0               WS01
Financials               0               WS01
IPC$            2147483651 Remote IPC    WS01
Old_reports              0               WS01
```

## Network Shares ACL

```pwsh
PS C:\Users\htb-student> Get-PathAcl "\\WS01\Client_Invoices"
```

```output title="Output" hl_lines="2 8 14 20"
Path              : \\WS01\Client_Invoices
FileSystemRights  : Read
IdentityReference : Local System
IdentitySID       : S-1-5-18
AccessControlType : Allow

Path              : \\WS01\Client_Invoices
FileSystemRights  : Read
IdentityReference : BUILTIN\Administrators
IdentitySID       : S-1-5-32-544
AccessControlType : Allow

Path              : \\WS01\Client_Invoices
FileSystemRights  : Synchronize,DeleteChild
IdentityReference : INLANEFREIGHT\Domain Users
IdentitySID       : S-1-5-21-2974783224-3764228556-2640795941-513
AccessControlType : Allow

Path              : \\WS01\Client_Invoices
FileSystemRights  : Read
IdentityReference : INLANEFREIGHT\mrb3n
IdentitySID       : S-1-5-21-2974783224-3764228556-2640795941-1121
AccessControlType : Allow
```
