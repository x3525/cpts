# SNMP

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 161 | Simple Network Management Protocol (SNMP) |
| 162 | Simple Network Management Protocol Trap (SNMPTRAP) |

## Object Identifiers

```sh
my@attack:~$ snmpwalk 10.129.95.102 -v2c -c public
```

## Community Strings

```sh
my@attack:~$ onesixtyone 10.129.95.102 -c /usr/local/share/wordlists/SecLists/Discovery/SNMP/snmp.txt
```

```output title="Output"
Scanning 1 hosts, 3219 communities
10.129.95.102 [public] Linux NIX02 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
10.129.95.102 [public] Linux NIX02 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
```

## Brute Forcing Object Identifiers

```sh
my@attack:~$ braa public@10.129.95.102:".1.3.6.*"
```
