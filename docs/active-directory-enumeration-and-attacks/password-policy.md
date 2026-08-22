# Password Policy

## Linux

### CrackMapExec

```sh
htb-student@ea-attack01:~$ crackmapexec smb 172.16.5.5 --pass-pol -u '' -p ''
```

### CrackMapExec (Credentialed)

```sh
htb-student@ea-attack01:~$ crackmapexec smb 172.16.5.5 --pass-pol -u 'svc_qualys' -p 'security#1'
```

```output title="Output" hl_lines="4 14"
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\svc_qualys:security#1 (Pwn3d!)
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Dumping password info for domain: INLANEFREIGHT
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Minimum password length: 8
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Password history length: 24
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Maximum password age: Not Set
SMB         172.16.5.5      445    ACADEMY-EA-DC01
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Password Complexity Flags: 000001
SMB         172.16.5.5      445    ACADEMY-EA-DC01      Domain Refuse Password Change: 0
SMB         172.16.5.5      445    ACADEMY-EA-DC01      Domain Password Store Cleartext: 0
SMB         172.16.5.5      445    ACADEMY-EA-DC01      Domain Password Lockout Admins: 0
SMB         172.16.5.5      445    ACADEMY-EA-DC01      Domain Password No Clear Change: 0
SMB         172.16.5.5      445    ACADEMY-EA-DC01      Domain Password No Anon Change: 0
SMB         172.16.5.5      445    ACADEMY-EA-DC01      Domain Password Complex: 1
SMB         172.16.5.5      445    ACADEMY-EA-DC01
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Minimum password age: 1 day 4 minutes
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Reset Account Lockout Counter: 30 minutes
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Locked Account Duration: 30 minutes
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Account Lockout Threshold: 5
SMB         172.16.5.5      445    ACADEMY-EA-DC01  Forced Log off Time: Not Set
```

### LDAP Anonymous Bind

```sh
htb-student@ea-attack01:~$ ldapsearch -H ldap://172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```

```output title="Output" hl_lines="7 10"
forceLogoff: -9223372036854775808
lockoutDuration: -18000000000
lockOutObservationWindow: -18000000000
lockoutThreshold: 5
maxPwdAge: -9223372036854775808
minPwdAge: -864000000000
minPwdLength: 8
modifiedCountAtLastProm: 0
nextRid: 1002
pwdProperties: 1
pwdHistoryLength: 24
```

### SMB NULL Session

#### rpcclient

```sh
htb-student@ea-attack01:~$ rpcclient -N -U "" 172.16.5.5 -c "getdompwinfo"
```

```output title="Output"
min_password_length: 8
password_properties: 0x00000001
        DOMAIN_PASSWORD_COMPLEX
```

#### enum4linux

```sh
htb-student@ea-attack01:~$ enum4linux -P 172.16.5.5
```

```output title="Output" hl_lines="50 60"
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Thu May 22 15:42:56 2025

 ==========================
|    Target Information    |
 ==========================
Target ........... 172.16.5.5
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================
|    Enumerating Workgroup/Domain on 172.16.5.5    |
 ==================================================
[+] Got domain/workgroup name: INLANEFREIGHT

 ===================================
|    Session Check on 172.16.5.5    |
 ===================================
[+] Server 172.16.5.5 allows sessions using username '', password ''

 =========================================
|    Getting domain SID for 172.16.5.5    |
 =========================================
Domain Name: INLANEFREIGHT
Domain Sid: S-1-5-21-3842939050-3880317879-2865463114
[+] Host is part of a domain (not a workgroup)

 ==================================================
|    Password Policy Information for 172.16.5.5    |
 ==================================================


[+] Attaching to 172.16.5.5 using a NULL share

[+] Trying protocol 139/SMB...

        [!] Protocol failed: Cannot request session (Called Name:172.16.5.5)

[+] Trying protocol 445/SMB...

[+] Found domain(s):

        [+] INLANEFREIGHT
        [+] Builtin

[+] Password Info for Domain: INLANEFREIGHT

        [+] Minimum password length: 8
        [+] Password history length: 24
        [+] Maximum password age: Not Set
        [+] Password Complexity Flags: 000001

                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 1

        [+] Minimum password age: 1 day 4 minutes
        [+] Reset Account Lockout Counter: 30 minutes
        [+] Locked Account Duration: 30 minutes
        [+] Account Lockout Threshold: 5
        [+] Forced Log off Time: Not Set


[+] Retieved partial password policy with rpcclient:

Password Complexity: Enabled
Minimum Password Length: 8

enum4linux complete on Thu May 22 15:42:57 2025
```

#### enum4linux-ng

```sh
htb-student@ea-attack01:~$ enum4linux-ng.py -P 172.16.5.5 -oA inlanefreight
```

```output title="Output" hl_lines="65 69"
ENUM4LINUX - next generation

 ==========================
|    Target Information    |
 ==========================
[*] Target ........... 172.16.5.5
[*] Username ......... ''
[*] Random Username .. 'rdsqeuso'
[*] Password ......... ''
[*] Timeout .......... 5 second(s)

 ==================================
|    Service Scan on 172.16.5.5    |
 ==================================
[*] Checking SMB
[+] SMB is accessible on 445/tcp
[*] Checking SMB over NetBIOS
[+] SMB over NetBIOS is accessible on 139/tcp

 =======================================
|    SMB Dialect Check on 172.16.5.5    |
 =======================================
[*] Trying on 445/tcp
[+] Supported dialects and settings:
SMB 1.0: false
SMB 2.02: true
SMB 2.1: true
SMB 3.0: true
SMB1 only: false
Preferred dialect: SMB 3.0
SMB signing required: true

 =======================================
|    RPC Session Check on 172.16.5.5    |
 =======================================
[*] Check for null session
[+] Server allows session using username '', password ''
[*] Check for random user session
[-] Could not establish random user session: STATUS_LOGON_FAILURE

 =================================================
|    Domain Information via RPC for 172.16.5.5    |
 =================================================
[+] Domain: INLANEFREIGHT
[+] SID: S-1-5-21-3842939050-3880317879-2865463114
[+] Host is part of a domain (not a workgroup)

 =========================================================
|    Domain Information via SMB session for 172.16.5.5    |
 =========================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found domain information via SMB
NetBIOS computer name: ACADEMY-EA-DC01
NetBIOS domain name: INLANEFREIGHT
DNS domain: INLANEFREIGHT.LOCAL
FQDN: ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL

 =======================================
|    Policies via RPC for 172.16.5.5    |
 =======================================
[*] Trying port 445/tcp
[+] Found policy:
domain_password_information:
  pw_history_length: 24
  min_pw_length: 8
  min_pw_age: 1 day 4 minutes
  max_pw_age: not set
  pw_properties:
  - DOMAIN_PASSWORD_COMPLEX: true
  - DOMAIN_PASSWORD_NO_ANON_CHANGE: false
  - DOMAIN_PASSWORD_NO_CLEAR_CHANGE: false
  - DOMAIN_PASSWORD_LOCKOUT_ADMINS: false
  - DOMAIN_PASSWORD_PASSWORD_STORE_CLEARTEXT: false
  - DOMAIN_PASSWORD_REFUSE_PASSWORD_CHANGE: false
domain_lockout_information:
  lockout_observation_window: 30 minutes
  lockout_duration: 30 minutes
  lockout_threshold: 5
domain_logoff_information:
  force_logoff_time: not set

Completed after 5.26 seconds
```

## Windows

### [Net Commands](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/net-commands-on-operating-systems)

```pwsh
PS C:\Users\htb-student> net accounts
```

```output title="Output" hl_lines="4"
Force user logoff how long after time expires?:       Never
Minimum password age (days):                          1
Maximum password age (days):                          Unlimited
Minimum password length:                              8
Length of password history maintained:                24
Lockout threshold:                                    5
Lockout duration (minutes):                           30
Lockout observation window (minutes):                 30
Computer role:                                        SERVER
The command completed successfully.
```

### PowerView

```pwsh
PS C:\Users\htb-student> Get-DomainPolicy
```

```output title="Output" hl_lines="2"
Unicode        : @{Unicode=yes}
SystemAccess   : @{MinimumPasswordAge=1; MaximumPasswordAge=-1; MinimumPasswordLength=8; PasswordComplexity=1; PasswordHistorySize=24; LockoutBadCount=5; ResetLockoutCount=30; LockoutDuration=30; RequireLogonToChangePassword=0; ForceLogoffWhenHourExpire=0; ClearTextPassword=0; LSAAnonymousNameLookup=0}
KerberosPolicy : @{MaxTicketAge=10; MaxRenewAge=7; MaxServiceAge=600; MaxClockSkew=5; TicketValidateClient=1}
Version        : @{signature="$CHICAGO$"; Revision=1}
RegistryValues : @{MACHINE\System\CurrentControlSet\Control\Lsa\NoLMHash=System.Object[]; MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters\SupportedEncryptionTypes=System.Object[]}
Path           : \\INLANEFREIGHT.LOCAL\sysvol\INLANEFREIGHT.LOCAL\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Microsoft\Windows NT\SecEdit\GptTmpl.inf
GPOName        : {31B2F340-016D-11D2-945F-00C04FB984F9}
GPODisplayName : Default Domain Policy
```
