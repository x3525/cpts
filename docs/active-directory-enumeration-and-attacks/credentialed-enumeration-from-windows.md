# Credentialed Enumeration from Windows

## ActiveDirectory Module

### Get-ADDomain

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADDomain
```

```output title="Output" hl_lines="2 9 11"
AllowedDNSSuffixes                 : {}
ChildDomains                       : {LOGISTICS.INLANEFREIGHT.LOCAL}
ComputersContainer                 : CN=Computers,DC=INLANEFREIGHT,DC=LOCAL
DeletedObjectsContainer            : CN=Deleted Objects,DC=INLANEFREIGHT,DC=LOCAL
DistinguishedName                  : DC=INLANEFREIGHT,DC=LOCAL
DNSRoot                            : INLANEFREIGHT.LOCAL
DomainControllersContainer         : OU=Domain Controllers,DC=INLANEFREIGHT,DC=LOCAL
DomainMode                         : Windows2016Domain
DomainSID                          : S-1-5-21-3842939050-3880317879-2865463114
ForeignSecurityPrincipalsContainer : CN=ForeignSecurityPrincipals,DC=INLANEFREIGHT,DC=LOCAL
Forest                             : INLANEFREIGHT.LOCAL
InfrastructureMaster               : ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
LastLogonReplicationInterval       :
LinkedGroupPolicyObjects           : {cn={DDBB8574-E94E-4525-8C9D-ABABE31223D0},cn=policies,cn=system,DC=INLANEFREIGHT,DC=LOCAL, CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=INLANEFREIGHT,DC=LOCAL}
LostAndFoundContainer              : CN=LostAndFound,DC=INLANEFREIGHT,DC=LOCAL
ManagedBy                          :
Name                               : INLANEFREIGHT
NetBIOSName                        : INLANEFREIGHT
ObjectClass                        : domainDNS
ObjectGUID                         : 71e4ecd1-a9f6-4f55-8a0b-e8c398fb547a
ParentDomain                       :
PDCEmulator                        : ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
PublicKeyRequiredPasswordRolling   : True
QuotasContainer                    : CN=NTDS Quotas,DC=INLANEFREIGHT,DC=LOCAL
ReadOnlyReplicaDirectoryServers    : {}
ReplicaDirectoryServers            : {ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL}
RIDMaster                          : ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
SubordinateReferences              : {DC=LOGISTICS,DC=INLANEFREIGHT,DC=LOCAL, DC=ForestDnsZones,DC=INLANEFREIGHT,DC=LOCAL, DC=DomainDnsZones,DC=INLANEFREIGHT,DC=LOCAL, CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL}
SystemsContainer                   : CN=System,DC=INLANEFREIGHT,DC=LOCAL
UsersContainer                     : CN=Users,DC=INLANEFREIGHT,DC=LOCAL
```

### Get-ADUser

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -Filter {servicePrincipalName -ne "$null"} -Properties servicePrincipalName
```

```output title="Output" hl_lines="7 19 31 43 55 67 79 91 103 115 127 139 151"
DistinguishedName    : CN=adfs,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            : Sharepoint
Name                 : adfs
ObjectClass          : user
ObjectGUID           : 49b53bea-4bc4-4a68-b694-b806d9809e95
SamAccountName       : adfs
servicePrincipalName : {adfsconnect/azure01.inlanefreight.local}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5244
Surname              : Admin
UserPrincipalName    :

DistinguishedName    : CN=BACKUPAGENT,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            : Jessica
Name                 : BACKUPAGENT
ObjectClass          : user
ObjectGUID           : 2ec53e98-3a64-4706-be23-1d824ff61bed
SamAccountName       : backupagent
servicePrincipalName : {backupjob/veam001.inlanefreight.local}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5220
Surname              : Systemmailbox 8Cc370d3-822A-4Ab8-A926-Bb94bd0641a9
UserPrincipalName    :

DistinguishedName    : CN=certsvc,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            :
Name                 : certsvc
ObjectClass          : user
ObjectGUID           : a07e13fc-6041-4d6c-a861-4826499827f0
SamAccountName       : certsvc
servicePrincipalName : {http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5633
Surname              :
UserPrincipalName    :

DistinguishedName    : CN=krbtgt,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : False
GivenName            : Brian
Name                 : krbtgt
ObjectClass          : user
ObjectGUID           : befe5574-48a0-4c72-b042-7b22fe6431f5
SamAccountName       : krbtgt
servicePrincipalName : {kadmin/changepw}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-502
Surname              : Manley
UserPrincipalName    :

DistinguishedName    : CN=Dana Amundsen,OU=DevOps,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            : Dana
Name                 : Dana Amundsen
ObjectClass          : user
ObjectGUID           : 76e07150-6074-4090-9957-ab4984c52bbb
SamAccountName       : damundsen
servicePrincipalName : {MSSQLSvc/ACADEMY-EA-DB01.INLANEFREIGHT.LOCAL:1433, MSSQL/ACADEMY-EA-FILE}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-1176
Surname              : Amundsen
UserPrincipalName    : damundsen@inlanefreight.local

DistinguishedName    : CN=sqldev,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            : Sharepoint
Name                 : sqldev
ObjectClass          : user
ObjectGUID           : c8aa09cf-fe36-4ecc-a1a5-a5eb9fab443a
SamAccountName       : sqldev
servicePrincipalName : {MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5243
Surname              : Admin
UserPrincipalName    :

DistinguishedName    : CN=sqlprod,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            : Sharepoint
Name                 : sqlprod
ObjectClass          : user
ObjectGUID           : 99b016ce-09e4-4f41-8594-c92fcbe80dcf
SamAccountName       : sqlprod
servicePrincipalName : {MSSQLSvc/SPSJDB.inlanefreight.local:1433}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5241
Surname              : Admin
UserPrincipalName    :

DistinguishedName    : CN=sqlqa,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            : Sharepoint
Name                 : sqlqa
ObjectClass          : user
ObjectGUID           : 43fb9f15-8a0f-4218-9dd6-b5842e916eb4
SamAccountName       : sqlqa
servicePrincipalName : {MSSQLSvc/SQL-CL01-01inlanefreight.local:49351}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5242
Surname              : Admin
UserPrincipalName    :

DistinguishedName    : CN=SAPService,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            :
Name                 : SAPService
ObjectClass          : user
ObjectGUID           : 8c72ba18-0c06-47b1-9d58-7d05930f516a
SamAccountName       : SAPService
servicePrincipalName : {SAPService/srv01.inlanefreight.local}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-7104
Surname              :
UserPrincipalName    :

DistinguishedName    : CN=SOLARWINDSMONITOR,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            : Jessica
Name                 : SOLARWINDSMONITOR
ObjectClass          : user
ObjectGUID           : 656ab144-9c2d-4904-a68c-883cc5742fbc
SamAccountName       : solarwindsmonitor
servicePrincipalName : {sts/inlanefreight.local}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5221
Surname              : Systemmailbox 8Cc370d3-822A-4Ab8-A926-Bb94bd0641a9
UserPrincipalName    :

DistinguishedName    : CN=testspn,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            :
Name                 : testspn
ObjectClass          : user
ObjectGUID           : f3226648-dd92-4fcc-9c20-633b2ef4eb9b
SamAccountName       : testspn
servicePrincipalName : {testspn/kerberoast.inlanefreight.local}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5604
Surname              :
UserPrincipalName    :

DistinguishedName    : CN=testspn2,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            :
Name                 : testspn2
ObjectClass          : user
ObjectGUID           : f3311429-be6e-414b-8ed4-621a7d063e66
SamAccountName       : testspn2
servicePrincipalName : {testspn2/kerberoast.inlanefreight.local}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-5605
Surname              :
UserPrincipalName    :

DistinguishedName    : CN=svc_vmwaresso,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
Enabled              : True
GivenName            :
Name                 : svc_vmwaresso
ObjectClass          : user
ObjectGUID           : e1b74d40-0e70-4e28-ad74-9f8f741984ce
SamAccountName       : svc_vmwaresso
servicePrincipalName : {vmware/inlanefreight.local}
SID                  : S-1-5-21-3842939050-3880317879-2865463114-6603
Surname              :
UserPrincipalName    :
```

### Get-ADGroup

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADGroup -Filter * | Select-Object name
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
Contractors
Accounting
Engineering
Executives
Human Resources
Marketing
Operations
Project Management
Sales
Senior Management
Service Accounts
Information Technology
Management
Tier 1 Admins
Tier 2 Admins
Tier 3 Admins
Tier 4 Admins
Help Desk Level 1
Local Admins
Executive Assistants
CEO
CFO
CTO
Computer Group Management
Exchange Administrator
Exchange User Management
Groups Management
High-Impact Server Management
Low-Impact Server Management
Medium-Impact Server Management
Mission-Critical Server Management
Lync Administrator
RBAC Management
Restricted Users Management
Role Group Management
Servers Management
Skype User Management
Standard Computers Management
Tier Admin Users Management
Fileshare Management
Distribution Group Management
GPO Management
Standard Users Management
Users Management
Print Service Management
Finance
Purchasing
Shipping
Warehouse
File Share Admin
File Share F Drive
File Share G Drive
File Share H Drive
File Share J Drive
Printer Access
MFP Access
ERP Admin
ERP Payment Access
ERP Sales
ERP Read
Sales Report Admin
Sales Report Read
Inventory Report Admin
Inventory Report Read
PLM Admin
PLM RW
PLM Read
Server Admin
Desktop Admin
Merch App Admin
Merch App Read
Shared Calendar Admin
Shared Calendar RW
Shared Calendar Read
VPN Users
Interns
Website Admin
Barracuda_all_access
Supervisors Warehouse
QA_users
Calendar Access
Nars360_users
Finance_billing_ilfreight
Nas Group
Front Desk
Billing
Finance_old
Barracuda_facebook_access
Barracuda_parked_sites
Barracuda_youtube_exempt
Rackspace_vpn_access
Collaboration_users
Ehr_group
Msp_users
Billing_users
Frontoffice_users
Executive_users
Communications_users
Facilities_users
Finance_mgt
Development_users
SQL Admins
SQL Dev
SQL QA
SQL Servers
IT Security
Network Ops
Secadmins
Temp Employees
MSSP Connect
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
Dev Accounts
```

### Get-ADGroupMember

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADGroupMember -Identity "Backup Operators"
```

```output title="Output" hl_lines="5"
distinguishedName : CN=BACKUPAGENT,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
name              : BACKUPAGENT
objectClass       : user
objectGUID        : 2ec53e98-3a64-4706-be23-1d824ff61bed
SamAccountName    : backupagent
SID               : S-1-5-21-3842939050-3880317879-2865463114-5220
```

## PowerView

### Get-DomainUser

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Domain INLANEFREIGHT.LOCAL -Identity mmorgan
```

```output title="Output" hl_lines="39"
postalcode                    : 11206
logoncount                    : 24
badpasswordtime               : 2/24/2022 3:10:06 PM
l                             : Brooklyn
department                    : Systems Engineer
objectclass                   : {top, person, organizationalPerson, user}
displayname                   : Matthew Morgan
lastlogontimestamp            : 2/27/2022 6:34:25 PM
userprincipalname             : mmorgan@inlanefreight.local
name                          : Matthew Morgan
lockouttime                   : 0
primarygroupid                : 513
objectsid                     : S-1-5-21-3842939050-3880317879-2865463114-1170
samaccountname                : mmorgan
logonhours                    : {255, 255, 255, 255...}
admincount                    : 1
codepage                      : 0
samaccounttype                : USER_OBJECT
accountexpires                : 12/31/1600 4:00:00 PM
cn                            : Matthew Morgan
whenchanged                   : 4/5/2022 7:34:54 PM
instancetype                  : 4
usncreated                    : 16965
objectguid                    : c8328fe9-d7c7-467b-a27a-d7596956ab6c
sn                            : Morgan
lastlogoff                    : 12/31/1600 4:00:00 PM
objectcategory                : CN=Person,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
distinguishedname             : CN=Matthew Morgan,OU=Server Admin,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
dscorepropagationdata         : {3/24/2022 3:58:07 PM, 3/24/2022 3:57:44 PM, 3/24/2022 3:56:18 PM, 3/24/2022 3:54:10 PM...}
givenname                     : Matthew
memberof                      : {CN=VPN Users,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Shared Calendar Read,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Printer Access,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=File Share H Drive,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL...}
lastlogon                     : 3/10/2022 11:48:06 AM
streetaddress                 : 4131 Turkey Pen Road
badpwdcount                   : 0
useraccountcontrol            : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, DONT_REQ_PREAUTH
whencreated                   : 10/27/2021 5:37:06 PM
countrycode                   : 0
pwdlastset                    : 4/5/2022 12:34:54 PM
msds-supportedencryptiontypes : 0
usnchanged                    : 3236140
```

### Get-DomainGroupMember

```pwsh
PS C:\Users\htb-student> Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

```output title="Output"
GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : svc_qualys
MemberDistinguishedName : CN=svc_qualys,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5613

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : sqldev
MemberDistinguishedName : CN=sqldev,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5243

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : sp-admin
MemberDistinguishedName : CN=Sharepoint Admin,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5228

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : freightlogisticsuser
MemberDistinguishedName : CN=FREIGHTLOGISTICSUSER,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5223

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : proxyagent
MemberDistinguishedName : CN=PROXYAGENT,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5222

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : solarwindsmonitor
MemberDistinguishedName : CN=SOLARWINDSMONITOR,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5221

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : backupagent
MemberDistinguishedName : CN=BACKUPAGENT,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5220

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : nagiosagent
MemberDistinguishedName : CN=NAGIOSAGENT,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5218

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : ldap.agent
MemberDistinguishedName : CN=LDAP.AGENT,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5216

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : clusteragent
MemberDistinguishedName : CN=clustergent,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5215

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : svc_sccm
MemberDistinguishedName : CN=Jessica Systemmailbox 8Cc370d3-822A-4Ab8-A926-Bb94bd0641a9,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5214

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : mrb3n
MemberDistinguishedName : CN=mrb3n,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-5213

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : Secadmins
MemberDistinguishedName : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : group
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-4112

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : coultle
MemberDistinguishedName : CN=Betty Turcotte,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1973

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : thisfic
MemberDistinguishedName : CN=Mary Clifton,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1972

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : betion
MemberDistinguishedName : CN=Ruby Cropper,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1971

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : grewle
MemberDistinguishedName : CN=Danielle Hawkins,OU=Operations,OU=Logistics-HK,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1970

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : ressoare
MemberDistinguishedName : CN=Melissa Jason,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1969

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : pratch
MemberDistinguishedName : CN=Johnnie Munoz,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1968

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : buithe
MemberDistinguishedName : CN=Christopher Taylor,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1967

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : fastally
MemberDistinguishedName : CN=Matthew Mackey,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1966

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : spong1990
MemberDistinguishedName : CN=Maggie Jablonski,OU=Operations,OU=Logistics-HK,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1965

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Secadmins
GroupDistinguishedName  : CN=Secadmins,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : intinted
MemberDistinguishedName : CN=Charlie Obando,OU=Operations,OU=Logistics-LAX,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1964

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : jhermann
MemberDistinguishedName : CN=John Hermann,OU=IT Admins,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1180

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : bross
MemberDistinguishedName : CN=Betty Ross,OU=IT Admins,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1179

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : dclick
MemberDistinguishedName : CN=Dorothy Click,OU=Server Admin,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1171

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : mmorgan
MemberDistinguishedName : CN=Matthew Morgan,OU=Server Admin,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1170

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : lab_adm
MemberDistinguishedName : CN=lab_adm,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-1001

GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Domain Admins
GroupDistinguishedName  : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : INLANEFREIGHT.LOCAL
MemberName              : administrator
MemberDistinguishedName : CN=Administrator,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
MemberObjectClass       : user
MemberSID               : S-1-5-21-3842939050-3880317879-2865463114-500
```

### Test-AdminAccess

```pwsh
PS C:\Users\htb-student> Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```

```output title="Output"
ComputerName    IsAdmin
------------    -------
ACADEMY-EA-MS01   False
```

## Snaffler

```pwsh
PS C:\Users\htb-student> C:\Tools\Snaffler.exe -d INLANEFREIGHT.LOCAL -s -v Data
```

```output title="Output" hl_lines="29"
2025-05-23 21:41:20 -07:00 [Share] {Black}(\\ACADEMY-EA-MS01.INLANEFREIGHT.LOCAL\ADMIN$)
2025-05-23 21:41:20 -07:00 [Share] {Black}(\\ACADEMY-EA-MS01.INLANEFREIGHT.LOCAL\C$)
2025-05-23 21:41:20 -07:00 [Share] {Green}(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares)
2025-05-23 21:41:20 -07:00 [Share] {Green}(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\User Shares)
2025-05-23 21:41:20 -07:00 [Share] {Green}(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\ZZZ_archive)
2025-05-23 21:41:43 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.kdb$|289B|3/31/2022 12:09:22 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Infosec\GroupBackup.kdb) .kdb
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.key$|298B|3/31/2022 12:05:10 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Infosec\ProtectStep.key) .key
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.keychain$|295B|3/31/2022 12:08:42 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Infosec\SetStep.keychain) .keychain
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.key$|299B|3/31/2022 12:05:33 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Infosec\ShowReset.key) .key
2025-05-23 21:41:43 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.ppk$|275B|3/31/2022 12:04:40 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Infosec\StopTrace.ppk) .ppk
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.keypair$|278B|3/31/2022 12:09:09 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Infosec\UnprotectConvertTo.keypair) .keypair
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.key$|301B|3/31/2022 12:09:17 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Infosec\WaitClear.key) .key
2025-05-23 21:41:43 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.kwallet$|302B|3/31/2022 12:04:45 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Infosec\WriteUse.kwallet) .kwallet
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.sqldump$|310B|3/31/2022 12:05:02 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\AddPublish.sqldump) .sqldump
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.sqldump$|312B|3/31/2022 12:05:30 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\DenyRedo.sqldump) .sqldump
2025-05-23 21:41:43 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.tblk$|280B|3/31/2022 12:05:17 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\ExportJoin.tblk) .tblk
2025-05-23 21:41:43 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.tblk$|279B|3/31/2022 12:05:25 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\FindConnect.tblk) .tblk
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.mdf$|305B|3/31/2022 12:09:27 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\FormatShow.mdf) .mdf
2025-05-23 21:41:43 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.psafe3$|301B|3/31/2022 12:09:33 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\GetUpdate.psafe3) .psafe3
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.mdf$|299B|3/31/2022 12:09:14 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\LockConfirm.mdf) .mdf
2025-05-23 21:41:43 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.psafe3$|275B|3/31/2022 12:08:50 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\RedoPop.psafe3) .psafe3
2025-05-23 21:41:43 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.cscfg$|309B|3/31/2022 12:04:57 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\RequestUnprotect.cscfg) .cscfg
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.sqldump$|318B|3/31/2022 12:09:01 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\ResolveExpand.sqldump) .sqldump
2025-05-23 21:41:43 -07:00 [File] {Red}<KeepExtExactRed|R|^\.sqldump$|302B|3/31/2022 12:08:58 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\RestoreCopy.sqldump) .sqldump
2025-05-23 21:41:44 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.tblk$|301B|3/31/2022 12:05:15 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\SubmitConvertFrom.tblk) .tblk
2025-05-23 21:41:44 -07:00 [File] {Red}<KeepConfigRegexRed|R|connectionstring[[:space:]]*=[[:space:]]*[\'\"][^\'\"].....*|253B|3/31/2022 12:12:43 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\web.config) <?xml version="1.0" encoding="utf-8"?>
<configuration>
  <connectionStrings>
    <add name="myConnectionString" connectionString="server=ACADEMY-EA-DB01;database=Employees;uid=sa;password=ILFREIGHTDB01!;" />
  </connectionStrings>
</configuration>
2025-05-23 21:41:44 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.psafe3$|305B|3/31/2022 12:09:41 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Development\WriteStart.psafe3) .psafe3
2025-05-23 21:41:44 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.vhdx$|282B|3/31/2022 12:05:07 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Systems\DB01-BACKUP.vhdx) .vhdx
2025-05-23 21:41:44 -07:00 [File] {Red}<KeepExtExactRed|R|^\.ova$|306B|3/31/2022 12:08:45 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Systems\NewCopy.ova) .ova
2025-05-23 21:41:44 -07:00 [File] {Red}<KeepExtExactRed|R|^\.wim$|289B|3/31/2022 12:09:38 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Systems\NewOptimize.wim) .wim
2025-05-23 21:41:44 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.ovpn$|288B|3/31/2022 12:05:20 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Systems\StepSync.ovpn) .ovpn
2025-05-23 21:41:44 -07:00 [File] {Red}<KeepExtExactRed|R|^\.pcap$|279B|3/31/2022 12:04:55 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Systems\UnprotectRestart.pcap) .pcap
2025-05-23 21:41:44 -07:00 [File] {Black}<KeepExtExactBlack|R|^\.vhdx$|285B|3/31/2022 12:09:11 PM>(\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\Department Shares\IT\Systems\WEB01-OLD.vhdx) .vhdx
```

## SharpHound

```pwsh
PS C:\Users\htb-student> C:\Tools\SharpHound.exe -c All
```

```output title="Output"
2025-05-23T21:48:55.3819178-07:00|INFORMATION|Resolved Collection Methods: Group, LocalAdmin, GPOLocalGroup, Session, LoggedOn, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote
2025-05-23T21:48:55.3974896-07:00|INFORMATION|Initializing SharpHound at 9:48 PM on 5/23/2025
2025-05-23T21:48:55.7412305-07:00|INFORMATION|Flags: Group, LocalAdmin, GPOLocalGroup, Session, LoggedOn, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote
2025-05-23T21:48:55.9756061-07:00|INFORMATION|Beginning LDAP search for INLANEFREIGHT.LOCAL
2025-05-23T21:49:26.4443979-07:00|INFORMATION|Status: 0 objects finished (+0 0)/s -- Using 65 MB RAM
2025-05-23T21:49:45.3037270-07:00|INFORMATION|Producer has finished, closing LDAP channel
2025-05-23T21:49:45.3037270-07:00|INFORMATION|LDAP channel closed, waiting for consumers
2025-05-23T21:49:56.4600299-07:00|INFORMATION|Status: 3793 objects finished (+3793 63.21667)/s -- Using 140 MB RAM
2025-05-23T21:50:04.0850148-07:00|INFORMATION|Consumers finished, closing output channel
2025-05-23T21:50:04.1162257-07:00|INFORMATION|Output channel closed, waiting for output task to complete
Closing writers
2025-05-23T21:50:04.2256004-07:00|INFORMATION|Status: 3809 objects finished (+16 56.01471)/s -- Using 143 MB RAM
2025-05-23T21:50:04.2256004-07:00|INFORMATION|Enumeration finished in 00:01:08.2556442
2025-05-23T21:50:04.5693512-07:00|INFORMATION|SharpHound Enumeration Completed at 9:50 PM on 5/23/2025! Happy Graphing!
```
