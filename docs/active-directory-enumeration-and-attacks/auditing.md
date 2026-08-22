# Auditing

## Sysinternals [ADExplorer](https://learn.microsoft.com/en-us/sysinternals/downloads/adexplorer)

1. Active Directory Explorer
    * Connect to: INLANEFREIGHT.LOCAL
    * User: htb-student
    * Password: Academy_student_AD!
2. File
3. Create Snapshot
    * Specify the path to the snapshot file: INLANEFREIGHT.dat
    * Throttle maximum server utilization: 100%

## [PingCastle](https://www.pingcastle.com/)

```pwsh
PS C:\Users\htb-student> C:\Tools\PingCastle\PingCastle.exe --healthcheck --level Full
```

```output title="Output"
Free Edition of PingCastle 3.2.0 - Not for commercial use
Starting the task: Perform analysis for INLANEFREIGHT.LOCAL
[8:57:25 AM] Getting domain information (INLANEFREIGHT.LOCAL)
[8:57:25 AM] Gathering general data
[8:57:26 AM] This domain contains approximatively 4284 objects
[8:57:26 AM] Gathering user data
[8:57:26 AM] Gathering computer data
[8:57:27 AM] Gathering trust data
[8:57:45 AM] Gathering privileged group and permissions data
[8:57:45 AM] - Initialize
[8:57:45 AM] - Searching for critical and infrastructure objects
[8:57:45 AM] - Collecting objects - Iteration 1
[8:57:46 AM] - Collecting objects - Iteration 2
[8:57:46 AM] - Collecting objects - Iteration 3
[8:57:46 AM] - Collecting objects - Iteration 4
[8:57:46 AM] - Collecting objects - Iteration 5
[8:57:46 AM] - Collecting objects - Iteration 6
[8:57:46 AM] - Collecting objects - Iteration 7
[8:57:46 AM] - Completing object collection
[8:57:46 AM] - Export completed
[8:57:46 AM] Gathering delegation data
[8:57:46 AM] Gathering gpo data
[8:57:47 AM] Gathering pki data
[8:59:32 AM] Gathering sccm data
[8:59:32 AM] Gathering exchange data
[8:59:32 AM] Gathering anomaly data
[8:59:32 AM] Gathering dns data
[8:59:33 AM] Gathering WSUS data
[8:59:33 AM] Gathering MSOL data
[8:59:33 AM] Gathering domain controller data (including null session) (including RPC tests)
[9:00:05 AM] Gathering network data
[9:00:05 AM] Computing risks
[9:00:05 AM] Export completed
[9:00:05 AM] Generating html report
[9:00:05 AM] Generating xml file for consolidation report
[9:00:05 AM] Export level is Full
[9:00:06 AM] Done
Task Perform analysis for INLANEFREIGHT.LOCAL completed
```

## [ADRecon](https://github.com/adrecon/ADRecon)

```pwsh
PS C:\Users\htb-student> C:\Tools\ADRecon\ADRecon.ps1
```

```output title="Output" hl_lines="32"
[*] ADRecon v1.1 by Prashant Mahajan (@prashant3535)
[*] Running on INLANEFREIGHT.LOCAL\ACADEMY-EA-MS01 - Member Server
[*] Commencing - 05/21/2025 09:16:40
[-] Domain
[-] Forest
[-] Trusts
[-] Sites
[-] Subnets
[-] Default Password Policy
[-] Fine Grained Password Policy - May need a Privileged Account
[-] Domain Controllers
[-] Users - May take some time
[-] User SPNs
[-] PasswordAttributes - Experimental
[-] Groups - May take some time
[-] Group Memberships - May take some time
[-] OrganizationalUnits (OUs)
[-] GPOs
[-] gPLinks - Scope of Management (SOM)
[-] DNS Zones and Records
[-] Printers
[-] Computers - May take some time
[-] Computer SPNs
[-] LAPS - Needs Privileged Account
WARNING: [*] LAPS is not implemented.
[-] BitLocker Recovery Keys - Needs Privileged Account
[-] ACLs - May take some time
WARNING: [*] SACLs - Currently, the module is only supported with LDAP.
[-] GPOReport - May take some time
[*] Total Execution Time (mins): 3.23
[*] Output Directory: C:\Users\htb-student\ADRecon-Report-20250521091640
WARNING: [Get-ADRExcelComObj] Excel does not appear to be installed. Skipping generation of ADRecon-Report.xlsx. Use the -GenExcel parameter to generate the ADRecon-Report.xslx on a host with Microsoft Excel installed.
```

## [DPAT](https://github.com/clr2of8/DPAT)

### Dumping Hashes

!!! warning

    Gerekli dosyalar:

    * ntds.dit
    * SYSTEM

    DC üzerinde çalıştır:

    ```pwsh
    PS C:\Users\Administrator> ntdsutil "ac i ntds" "ifm" "create full C:\temp" q q
    ```

```sh
my@attack:~$ secretsdump.py -ntds "Active Directory/ntds.dit" -system "registry/SYSTEM" -security SECURITY LOCAL -outputfile customer
```

### Dumping Hashes (DCSync)

```sh
my@attack:~$ secretsdump.py 'INLANEFREIGHT.LOCAL'/'Administrator':'Repeat09'@172.16.8.3 -outputfile customer
```

### Hash Cracking

```sh
my@attack:~$ hashcat --hwmon-disable -a 0 -m 1000 customer.ntds /usr/local/share/wordlists/rockyou.txt
```

### Generating the Statistics

```sh
my@attack:~$ dpat.py -n customer.ntds -c ~/.local/share/hashcat/hashcat.potfile
```
