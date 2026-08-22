# LDAP Search Filters

## Operators

| OPERATOR | DESCRIPTION |
|---|---|
| = | Eşit |
| ~= | Yaklaşık olarak eşit |
| <= | Küçük veya eşit |
| >= | Büyük veya eşit |
| & | Ve |
| &#124; | Veya |
| ! | Değil |

[Polish notasyonu](https://en.wikipedia.org/wiki/Polish_notation) kullanılır:

```text title="Example"
(|(&(objectClass=user)(cn=andy*))(&(objectClass=user)(cn=steve*)))
```

Bu ifade aşağıdaki ifadeye karşılık gelir:

```text title="Example"
( (objectClass=user) & (cn=andy*) ) | ( (objectClass=user) & (cn=steve*) )
```

## Special Characters

| CHARACTER | ESCAPE SEQUENCE |
|---|---|
| * | \2a |
| ( | \28 |
| ) | \29 |
| \ | \5c |
| NUL | \00 |
| / | \2f |

## Scope

| NAME | LEVEL | DESCRIPTION |
|---|---|---|
| Base | 0 | SearchBase ögesi için sorgu gerçekleştir. |
| OneLevel | 1 | SearchBase ögesinin altındaki ögeler için sorgu gerçekleştir. |
| Subtree | 2 | SearchBase ögesi ve altındaki ögeler için sorgu gerçekleştir. |

SearchScope 0 olarak sorgu yaparsak herhangi bir kullanıcı dönmez:

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -SearchBase "OU=Employees,DC=INLANEFREIGHT,DC=LOCAL" -SearchScope 0 -Filter *
```

SearchScope 1 olarak sorgu yaparsak Amelia Matthews kullanıcısı döner:

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -SearchBase "OU=Employees,DC=INLANEFREIGHT,DC=LOCAL" -SearchScope 1 -Filter *
```

```output title="Output"
DistinguishedName : CN=Amelia Matthews,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
Enabled           : True
GivenName         : amelia
Name              : Amelia Matthews
ObjectClass       : user
ObjectGUID        : 3f04328f-eb2e-487c-85fe-58dd598159c0
SamAccountName    : amelia.matthews
SID               : S-1-5-21-2974783224-3764228556-2640795941-1412
Surname           : matthews
UserPrincipalName : amelia.matthews@inlanefreight
```

SearchScope 2 olarak sorgu yaparsak 970 adet kullanıcı döner:

```pwsh
PS C:\Users\htb-student> (ActiveDirectory\Get-ADUser -SearchBase "OU=Employees,DC=INLANEFREIGHT,DC=LOCAL" -SearchScope 2 -Filter *).count
```

```output title="Output"
970
```

## Matching Rules

| MATCHING RULE | STRING IDENTIFIER |
|---|---|
| 1.2.840.113556.1.4.803 | LDAP_MATCHING_RULE_BIT_AND |
| 1.2.840.113556.1.4.804 | LDAP_MATCHING_RULE_BIT_OR |
| 1.2.840.113556.1.4.1941 | LDAP_MATCHING_RULE_IN_CHAIN |

## UAC

| DECIMAL | HEXADECIMAL | PROPERTY FLAG |
|---|---|---|
| 1 | 0x0001 | SCRIPT |
| 2 | 0x0002 | ACCOUNTDISABLE |
| 8 | 0x0008 | HOMEDIR_REQUIRED |
| 16 | 0x0010 | LOCKOUT |
| 32 | 0x0020 | PASSWD_NOTREQD |
| 64 | 0x0040 | PASSWD_CANT_CHANGE |
| 128 | 0x0080 | ENCRYPTED_TEXT_PWD_ALLOWED |
| 256 | 0x0100 | TEMP_DUPLICATE_ACCOUNT |
| 512 | 0x0200 | NORMAL_ACCOUNT |
| 2048 | 0x0800 | INTERDOMAIN_TRUST_ACCOUNT |
| 4096 | 0x1000 | WORKSTATION_TRUST_ACCOUNT |
| 8192 | 0x2000 | SERVER_TRUST_ACCOUNT |
| 65536 | 0x10000 | DONT_EXPIRE_PASSWORD |
| 131072 | 0x20000 | MNS_LOGON_ACCOUNT |
| 262144 | 0x40000 | SMARTCARD_REQUIRED |
| 524288 | 0x80000 | TRUSTED_FOR_DELEGATION |
| 1048576 | 0x100000 | NOT_DELEGATED |
| 2097152 | 0x200000 | USE_DES_KEY_ONLY |
| 4194304 | 0x400000 | DONT_REQ_PREAUTH |
| 8388608 | 0x800000 | PASSWORD_EXPIRED |
| 16777216 | 0x1000000 | TRUSTED_TO_AUTH_FOR_DELEGATION |
| 67108864 | 0x04000000 | PARTIAL_SECRETS_ACCOUNT |

## AD Schema

* [All Attributes](https://learn.microsoft.com/en-us/windows/win32/adschema/attributes-all)
* [All Classes](https://learn.microsoft.com/en-us/windows/win32/adschema/classes-all)

## Example LDAP Filters

### Disabled User Accounts

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -LDAPFilter "(userAccountControl:1.2.840.113556.1.4.803:=2)" | Select-Object name
```

```output title="Output"
name
----
Guest
DefaultAccount
krbtgt
Caroline Ali
Luke Gibbons
Exchange Online-ApplicationAccount
SystemMailbox{1f05a927-35b9-4cc9-bbe1-11e28cddb180}
SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}
SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}
DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852}
Migration.8f3e7716-2011-43e4-96b1-aba62d229136
FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042
SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201}
SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA}
SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9}
```

### PASSWD_NOTREQD

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -LDAPFilter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))(adminCount=1)" -Properties * | Select-Object name,memberof | fl
```

```output title="Output"
name     : Jenna Smith
memberof : {CN=Schema Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL}

name     : Harry Jones
memberof : {CN=Network Operations,CN=Users,DC=INLANEFREIGHT,DC=LOCAL, CN=Network Team,CN=Users,DC=INLANEFREIGHT,DC=LOCAL, CN=Help Desk,OU=Microsoft Exchange Security Groups,DC=INLANEFREIGHT,DC=LOCAL, CN=Security Operations,CN=Users,DC=INLANEFREIGHT,DC=LOCAL...}
```

### Description Field

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -LDAPFilter "(&(objectCategory=user)(description=*))" -Properties * | Select-Object samaccountname,description
```

### Nested Group Membership

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADGroup -LDAPFilter "(member:1.2.840.113556.1.4.1941:=CN=Harry Jones,OU=Network Ops,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL)" | Select-Object name
```

```output title="Output"
name
----
Administrators
Backup Operators
Domain Admins
Denied RODC Password Replication Group
LAPS Admins
Security Operations
Help Desk
Network Team
Network Operations
```
