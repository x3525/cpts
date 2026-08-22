# Active Directory Search Filters

## Filtering

Filtre için {} kullanılabilir:

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -Filter {name -eq "sally jones"}
```

Filtre için "" kullanılabilir:

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -Filter "name -eq 'sally jones'"
```

Filtre için '' kullanılabilir:

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -Filter 'name -eq "sally jones"'
```

## Filter for SQL

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADComputer -Filter {DNSHostName -like "SQL*"}
```

```output title="Output"
DistinguishedName : CN=SQL01,OU=SQL Servers,OU=Servers,DC=INLANEFREIGHT,DC=LOCAL
DNSHostName       : SQL01.INLANEFREIGHT.LOCAL
Enabled           : True
Name              : SQL01
ObjectClass       : computer
ObjectGUID        : 42cc9264-1655-4bfa-b5f9-21101afb33d0
SamAccountName    : SQL01$
SID               : S-1-5-21-2974783224-3764228556-2640795941-1104
UserPrincipalName :
```

## Filter Administrative Groups

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADGroup -Filter {adminCount -eq 1} | Select-Object name
```

```output title="Output"
name
----
Administrators
Print Operators
Backup Operators
Replicator
Domain Controllers
Schema Admins
Enterprise Admins
Domain Admins
Server Operators
Account Operators
Read-only Domain Controllers
Security Operations
```

## Subject to AS-REP Roasting

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -Filter {DoesNotRequirePreAuth -eq $true -and adminCount -eq 1}
```

```output title="Output" hl_lines="7"
DistinguishedName : CN=Jenna Smith,OU=Server Team,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
Enabled           : True
GivenName         : jenna
Name              : Jenna Smith
ObjectClass       : user
ObjectGUID        : ea3c930f-aa8e-4fdc-987c-4a9ee1a75409
SamAccountName    : jenna.smith
SID               : S-1-5-21-2974783224-3764228556-2640795941-1999
Surname           : smith
UserPrincipalName : jenna.smith@inlanefreight
```

## Subject to Kerberoasting

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -Filter {adminCount -eq 1} -Properties * | ? servicePrincipalName -ne $null | Select-Object samaccountname,memberof,serviceprincipalname | fl
```

```output title="Output" hl_lines="1 5"
samaccountname       : krbtgt
memberof             : {CN=Denied RODC Password Replication Group,CN=Users,DC=INLANEFREIGHT,DC=LOCAL}
serviceprincipalname : {kadmin/changepw}

samaccountname       : sqlqa
memberof             : {CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL}
serviceprincipalname : {MSSQL_svc_qa/inlanefreight.local:1443}
```
