# Enumerating AD Users

## Number of Users

```pwsh
PS C:\Users\htb-student> (Get-DomainUser).count
```

```output title="Output"
1042
```

## Most Important Properties

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Domain INLANEFREIGHT.LOCAL -Identity harry.jones | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
```

```output title="Output"
name                 : Harry Jones
samaccountname       : harry.jones
description          :
memberof             : {CN=Network Operations,CN=Users,DC=INLANEFREIGHT,DC=LOCAL, CN=Network Team,CN=Users,DC=INLANEFREIGHT,DC=LOCAL, CN=Help Desk,OU=Microsoft Exchange Security Groups,DC=INLANEFREIGHT,DC=LOCAL, CN=Security Operations,CN=Users,DC=INLANEFREIGHT,DC=LOCAL...}
whencreated          : 7/27/2020 7:35:59 PM
pwdlastset           : 9/9/2020 11:24:47 PM
lastlogontimestamp   : 9/2/2020 9:54:53 PM
accountexpires       : 12/31/1600 4:00:00 PM
admincount           : 1
userprincipalname    : harry.jones@INLANEFREIGHT.LOCAL
serviceprincipalname :
mail                 : hjones@inlanefreight.local
useraccountcontrol   : PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD
```

## Exporting Users to a CSV File

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Domain INLANEFREIGHT.LOCAL -Identity * | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,mail,useraccountcontrol | Export-Csv users.csv -NoTypeInformation
```

## Unconstrained Delegation

```pwsh
PS C:\Users\htb-student> Get-DomainUser -LDAPFilter "(userAccountControl:1.2.840.113556.1.4.803:=524288)"
```

```output title="Output" hl_lines="7 36"
logoncount            : 0
badpasswordtime       : 12/31/1600 4:00:00 PM
distinguishedname     : CN=sqldev,OU=Service Accounts,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
objectclass           : {top, person, organizationalPerson, user}
name                  : sqldev
objectsid             : S-1-5-21-2974783224-3764228556-2640795941-1110
samaccountname        : sqldev
codepage              : 0
samaccounttype        : USER_OBJECT
accountexpires        : 12/31/1600 4:00:00 PM
countrycode           : 0
whenchanged           : 8/14/2020 1:30:37 PM
instancetype          : 4
objectguid            : f71224a5-baa7-4aec-bfe9-56778184dc63
lastlogon             : 12/31/1600 4:00:00 PM
lastlogoff            : 12/31/1600 4:00:00 PM
objectcategory        : CN=Person,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
dscorepropagationdata : {7/30/2020 3:09:16 AM, 7/30/2020 3:09:16 AM, 7/28/2020 1:45:00 AM, 7/28/2020 1:34:13 AM...}
serviceprincipalname  : {CIFS/roguecomputer.inlanefreight.local, MSSQL_svc_dev/inlanefreight.local:1443}
memberof              : CN=Protected Users,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
whencreated           : 7/27/2020 6:46:20 PM
badpwdcount           : 0
cn                    : sqldev
useraccountcontrol    : NORMAL_ACCOUNT, TRUSTED_FOR_DELEGATION
usncreated            : 14648
primarygroupid        : 513
pwdlastset            : 7/27/2020 11:46:20 AM
usnchanged            : 110783

logoncount            : 0
badpasswordtime       : 12/31/1600 4:00:00 PM
distinguishedname     : CN=sql-test,OU=Service Accounts,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
objectclass           : {top, person, organizationalPerson, user}
name                  : sql-test
objectsid             : S-1-5-21-2974783224-3764228556-2640795941-1116
samaccountname        : sql-test
codepage              : 0
samaccounttype        : USER_OBJECT
accountexpires        : 12/31/1600 4:00:00 PM
countrycode           : 0
whenchanged           : 9/10/2020 4:57:32 AM
instancetype          : 4
objectguid            : 98fc0c02-27f9-4267-bc12-b94eb6588418
lastlogon             : 12/31/1600 4:00:00 PM
lastlogoff            : 12/31/1600 4:00:00 PM
objectcategory        : CN=Person,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
dscorepropagationdata : {7/30/2020 3:09:16 AM, 7/30/2020 3:09:16 AM, 7/28/2020 1:45:00 AM, 7/28/2020 1:34:13 AM...}
serviceprincipalname  : MSSQL_svc_test/inlanefreight.local:1443
whencreated           : 7/27/2020 6:47:07 PM
badpwdcount           : 0
cn                    : sql-test
useraccountcontrol    : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, TRUSTED_FOR_DELEGATION
usncreated            : 14678
primarygroupid        : 513
pwdlastset            : 7/27/2020 11:47:07 AM
usnchanged            : 176565
```

## Constrained Delegation

```pwsh
PS C:\Users\htb-student> Get-DomainUser -TrustedToAuth -Properties samaccountname,useraccountcontrol,memberof
```

```output title="Output"
memberof                                                                                useraccountcontrol samaccountname
--------                                                                                ------------------ --------------
CN=Protected Users,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                       NORMAL_ACCOUNT sqlprod
                                                                      NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD svc-scan
                                                      PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD adam.jones
```

## Subject to AS-REP Roasting

```pwsh
PS C:\Users\htb-student> Get-DomainUser -KerberosPreauthNotRequired -Properties samaccountname,useraccountcontrol,memberof
```

```output title="Output"
samaccountname                                                     useraccountcontrol
--------------                                                     ------------------
ross.begum                           PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_REQ_PREAUTH
amber.smith    PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, DONT_REQ_PREAUTH
jenna.smith    PASSWD_NOTREQD, NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, DONT_REQ_PREAUTH
```

## Subject to Kerberoasting

```pwsh
PS C:\Users\htb-student> Get-DomainUser -SPN -Properties samaccountname,memberof,serviceprincipalname
```

```output title="Output"
serviceprincipalname                                                             memberof                                                                     samaccountname
--------------------                                                             --------                                                                     --------------
{CIFS/roguecomputer.inlanefreight.local, MSSQL_svc_dev/inlanefreight.local:1443} CN=Protected Users,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                        sqldev
HTTP/WSUS01.inlanefreight.local:8530                                                                                                                          WSUSupdatesvc
IIS_dev/inlanefreight.local:80                                                                                                                                adam.jones
kadmin/changepw                                                                  CN=Denied RODC Password Replication Group,CN=Users,DC=INLANEFREIGHT,DC=LOCAL krbtgt
MSSQL_svc_qa/inlanefreight.local:1443                                            CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                          sqlqa
MSSQL_svc_test/inlanefreight.local:1443                                                                                                                       sql-test
MSSQLSvc/sql01:1433                                                              CN=Protected Users,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                        sqlprod
scantest/inlanefreight.local                                                                                                                                  svc-scan
```

## Description Field

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Properties samaccountname,description | ? {$_.description -ne $null}
```

## Foreign Domain Group Membership

```pwsh
PS C:\Users\htb-student> Find-ForeignGroup
```

```output title="Output"
GroupDomain             : INLANEFREIGHT.LOCAL
GroupName               : Administrators
GroupDistinguishedName  : CN=Administrators,CN=Builtin,DC=INLANEFREIGHT,DC=LOCAL
MemberDomain            : LOGISTICS.INLANEFREIGHT.LOCAL
MemberName              : S-1-5-21-2940054386-2308160471-1560758553-1601
MemberDistinguishedName : CN=S-1-5-21-2940054386-2308160471-1560758553-1601,CN=Users,DC=LOGISTICS,DC=INLANEFREIGHT,DC=LOCAL
```

## Service Principal Name

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Domain INLANEFREIGHT.LOCAL -SPN | Select-Object samaccountname,memberof,serviceprincipalname | fl
```

```output title="Output"
samaccountname       : sqldev
memberof             : CN=Protected Users,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
serviceprincipalname : {CIFS/roguecomputer.inlanefreight.local, MSSQL_svc_dev/inlanefreight.local:1443}

samaccountname       : WSUSupdatesvc
memberof             :
serviceprincipalname : HTTP/WSUS01.inlanefreight.local:8530

samaccountname       : adam.jones
memberof             :
serviceprincipalname : IIS_dev/inlanefreight.local:80

samaccountname       : krbtgt
memberof             : CN=Denied RODC Password Replication Group,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
serviceprincipalname : kadmin/changepw

samaccountname       : sqlqa
memberof             : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
serviceprincipalname : MSSQL_svc_qa/inlanefreight.local:1443

samaccountname       : sql-test
memberof             :
serviceprincipalname : MSSQL_svc_test/inlanefreight.local:1443

samaccountname       : sqlprod
memberof             : CN=Protected Users,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
serviceprincipalname : MSSQLSvc/sql01:1433

samaccountname       : svc-scan
memberof             :
serviceprincipalname : scantest/inlanefreight.local
```

## Password Set Time

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Domain INLANEFREIGHT.LOCAL -Properties samaccountname,pwdlastset,lastlogon | Select-Object samaccountname,pwdlastset,lastlogon | ? {$_.pwdlastset -lt (Get-Date).addDays(-90)}
```

```output title="Output"
samaccountname      pwdlastset            lastlogon
--------------      ----------            ---------
sarah.lafferty      8/14/2020 4:06:13 AM  8/14/2020 4:06:37 AM
alison.herbert      9/9/2020 11:19:58 PM  9/9/2020 11:21:32 PM
harriet.richardson  8/25/2020 2:19:46 PM  8/25/2020 2:20:19 PM
linda.wells         8/25/2020 2:20:07 PM  8/25/2020 2:20:19 PM
amelia.matthews     9/3/2020 12:25:23 PM  9/3/2020 12:26:26 PM
harry.jones         9/9/2020 11:24:47 PM  9/3/2020 11:43:12 AM
wilford.stewart     8/14/2020 4:05:20 AM  8/14/2020 4:05:49 AM
trisha.duran        8/14/2020 4:03:48 AM  8/14/2020 4:04:16 AM
```
