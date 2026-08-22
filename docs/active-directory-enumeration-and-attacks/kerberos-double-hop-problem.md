# Kerberos Double Hop Problem

| CONNECTION | DOUBLE HOP PROBLEM |
|---|---|
| WinRM | + |
| RDP ||

## WinRM

### Evil-WinRM

```sh
htb-student@ea-attack01:~$ evil-winrm -i 172.16.5.25 -u 'forend' -p 'Klmcargo2'
```

### Evil-WinRM Double Hop Problem

```pwsh
*Evil-WinRM* PS C:\Users\forend.INLANEFREIGHT\Documents> Get-DomainUser -Identity forend -Properties badpasswordtime
```

```output title="Output"
Cannot convert argument "fileTime", with value: "System.Byte[]", for "FromFileTime" to type "System.Int64": "Cannot convert the "System.Byte[]" value of type "System.Byte[]" to type "System.Int64"."
At C:\Tools\PowerView.ps1:3224 char:21
+ ...             $ObjectProperties[$_] = ([datetime]::FromFileTime(($Prope ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodException
    + FullyQualifiedErrorId : MethodArgumentConversionInvalidCastArgument
```

### Workaround

```pwsh
*Evil-WinRM* PS C:\Users\forend.INLANEFREIGHT\Documents> $pass = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force
*Evil-WinRM* PS C:\Users\forend.INLANEFREIGHT\Documents> $cred = New-Object System.Management.Automation.PSCredential("INLANEFREIGHT\forend", $pass)
*Evil-WinRM* PS C:\Users\forend.INLANEFREIGHT\Documents> Get-DomainUser -Domain INLANEFREIGHT.HTB -Identity forend -Credential $cred -Properties badpasswordtime
```

```output title="Output"
badpasswordtime
---------------
4/5/2022 7:09:07 AM
```

## WinRM (GUI)

### Enter-PSSession

```pwsh
PS C:\Users\htb-student> Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential "INLANEFREIGHT\forend"
```

### Enter-PSSession Double Hop Problem

```pwsh
[ACADEMY-EA-MS01]: PS C:\Users\forend.INLANEFREIGHT\Documents> Get-DomainUser -Identity forend -Properties badpasswordtime
```

```output title="Output"
Cannot convert argument "fileTime", with value: "System.Byte[]", for "FromFileTime" to type "System.Int64": "Cannot convert the "System.Byte[]" value of type "System.Byte[]" to type "System.Int64"."
At C:\Tools\PowerView.ps1:3224 char:21
+ ...             $ObjectProperties[$_] = ([datetime]::FromFileTime(($Prope ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodException
    + FullyQualifiedErrorId : MethodArgumentConversionInvalidCastArgument
```

### Workaround (GUI)

#### Register-PSSessionConfiguration

```pwsh
[ACADEMY-EA-MS01]: PS C:\Users\forend.INLANEFREIGHT\Documents> Register-PSSessionConfiguration -Name forend-session -RunAsCredential "INLANEFREIGHT\forend"
```

#### Restart-Service

```pwsh
[ACADEMY-EA-MS01]: PS C:\Users\forend.INLANEFREIGHT\Documents> Restart-Service WinRM
```

#### New Session

```pwsh
PS C:\Users\htb-student> Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential "INLANEFREIGHT\forend" -ConfigurationName forend-session
```

### Reviewing Current Session Tickets

```pwsh
[ACADEMY-EA-MS01]: PS C:\Users\forend.INLANEFREIGHT\Documents> klist
```

```output title="Output" hl_lines="6"
Current LogonId is 0:0xaa60e

Cached Tickets: (1)

#0>     Client: forend @ INLANEFREIGHT.LOCAL
        Server: krbtgt/INLANEFREIGHT.LOCAL @ INLANEFREIGHT.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 5/26/2025 13:15:31 (local)
        End Time:   5/26/2025 23:15:31 (local)
        Renew Time: 6/2/2025 13:15:31 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called: ACADEMY-EA-DC01
```

### Problem Solved

```pwsh
[ACADEMY-EA-MS01]: PS C:\Users\forend.INLANEFREIGHT\Documents> Get-DomainUser -Identity forend -Properties badpasswordtime
```

```output title="Output"
badpasswordtime
---------------
4/5/2022 7:09:07 AM
```
