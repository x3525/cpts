# Enumerating AD Groups

## Domain Groups

```pwsh
PS C:\Users\htb-student> Get-DomainGroup -Properties name
```

```output title="Output"
name
----
Administrators
Users
Guests
Print Operators
Backup Operators
Replicator
Remote Desktop Users
Network Configuration Operators
Performance Monitor Users
Performance Log Users
Distributed COM Users
IIS_IUSRS
Cryptographic Operators
Event Log Readers
Certificate Service DCOM Access
RDS Remote Access Servers
RDS Endpoint Servers
RDS Management Servers
Hyper-V Administrators
Access Control Assistance Operators
Remote Management Users
System Managed Accounts Group
Storage Replica Administrators
Domain Computers
Domain Controllers
Schema Admins
Enterprise Admins
Cert Publishers
Domain Admins
Domain Users
Domain Guests
Group Policy Creator Owners
RAS and IAS Servers
Server Operators
Account Operators
Pre-Windows 2000 Compatible Access
Incoming Forest Trust Builders
Windows Authorization Access Group
Terminal Server License Servers
Allowed RODC Password Replication Group
Denied RODC Password Replication Group
Read-only Domain Controllers
Enterprise Read-only Domain Controllers
Cloneable Domain Controllers
Protected Users
Key Admins
Enterprise Key Admins
DnsAdmins
DnsUpdateProxy
LAPS Admins
Security Operations
Organization Management
Recipient Management
View-Only Organization Management
Public Folder Management
UM Management
Help Desk
Records Management
Discovery Management
Server Management
Delegated Setup
Hygiene Management
Compliance Management
Security Reader
Security Administrator
Exchange Servers
Exchange Trusted Subsystem
Managed Availability Servers
Exchange Windows Permissions
ExchangeLegacyInterop
Exchange Install Domain Servers
Network Team
Network Operations
```

## Domain Group Members

```pwsh
PS C:\Users\htb-student> Get-DomainGroupMember -Identity "Help Desk"
```

```output title="Output" hl_lines="5 14 23"
GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Help Desk
GroupDistinguishedName  : CN=Help Desk,OU=Microsoft Exchange Security Groups,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : harry.jones
MemberDistinguishedName : CN=Harry Jones,OU=Network Ops,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-2974783224-3764228556-2640795941-2040

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Help Desk
GroupDistinguishedName  : CN=Help Desk,OU=Microsoft Exchange Security Groups,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : amber.smith
MemberDistinguishedName : CN=Amber Smith,OU=Contractors,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-2974783224-3764228556-2640795941-1859

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Help Desk
GroupDistinguishedName  : CN=Help Desk,OU=Microsoft Exchange Security Groups,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : pamela.brown
MemberDistinguishedName : CN=Pamela Brown,OU=Server Team,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-2974783224-3764228556-2640795941-1190
```

## Protected Groups

```pwsh
PS C:\Users\htb-student> Get-DomainGroup -AdminCount
```

## Find-ManagedSecurityGroups

```pwsh
PS C:\Users\htb-student> Find-ManagedSecurityGroups | Select-Object GroupName
```

```output title="Output"
GroupName
---------
Security Operations
Organization Management
Recipient Management
View-Only Organization Management
Public Folder Management
UM Management
Help Desk
Records Management
Discovery Management
Server Management
Delegated Setup
Hygiene Management
Compliance Management
Security Reader
Security Administrator
```

## Get-DomainManagedSecurityGroup

```pwsh
PS C:\Users\htb-student> Get-DomainManagedSecurityGroup
```

```output title="Output" hl_lines="3"
GroupName                : Security Operations
GroupDistinguishedName   : CN=Security Operations,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
ManagerName              : joe.evans
ManagerDistinguishedName : CN=Joe Evans,OU=Security,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
ManagerType              : User
ManagerCanWrite          : UNKNOWN
```

## Local Groups

```pwsh
PS C:\Users\htb-student> Get-NetLocalGroup -ComputerName WS01 | Select-Object GroupName
```

```output title="Output"
GroupName
---------
Access Control Assistance Operators
Administrators
Backup Operators
Certificate Service DCOM Access
Cryptographic Operators
Distributed COM Users
Event Log Readers
Guests
Hyper-V Administrators
IIS_IUSRS
Network Configuration Operators
Performance Log Users
Performance Monitor Users
Power Users
Print Operators
RDS Endpoint Servers
RDS Management Servers
RDS Remote Access Servers
Remote Desktop Users
Remote Management Users
Replicator
Storage Replica Administrators
System Managed Accounts Group
Users
```

## Local Group Members

```pwsh
PS C:\Users\htb-student> Get-NetLocalGroupMember -ComputerName WS01
```

```output title="Output"
ComputerName : WS01
GroupName    : Administrators
MemberName   : WS01\Administrator
SID          : S-1-5-21-3098764391-2955872655-3533479253-500
IsGroup      : False
IsDomain     : False

ComputerName : WS01
GroupName    : Administrators
MemberName   : INLANEFREIGHT\Domain Admins
SID          : S-1-5-21-2974783224-3764228556-2640795941-512
IsGroup      : True
IsDomain     : True

ComputerName : WS01
GroupName    : Administrators
MemberName   : INLANEFREIGHT\harry.jones
SID          : S-1-5-21-2974783224-3764228556-2640795941-2040
IsGroup      : False
IsDomain     : True

ComputerName : WS01
GroupName    : Administrators
MemberName   : INLANEFREIGHT\Domain Users
SID          : S-1-5-21-2974783224-3764228556-2640795941-513
IsGroup      : True
IsDomain     : True

ComputerName : WS01
GroupName    : Administrators
MemberName   : INLANEFREIGHT\sqldev
SID          : S-1-5-21-2974783224-3764228556-2640795941-1110
IsGroup      : False
IsDomain     : True
```
