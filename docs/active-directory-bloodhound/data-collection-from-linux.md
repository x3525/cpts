# Data Collection from Linux

## Installing [BloodHound](https://github.com/dirkjanm/BloodHound.py) Python

```sh
my@attack:~$ pip3 install bloodhound
```

## Gathering Data

```sh
my@attack:~$ bloodhound-python -d INLANEFREIGHT.HTB -u 'htb-student' -p 'HTBRocks!' --nameserver 10.129.136.59 -c DCOnly
```

```sh
my@attack:~$ bloodhound-python -d INLANEFREIGHT.HTB -u 'htb-student' -p 'HTBRocks!' --nameserver 10.129.136.59 -c DCOnly --dns-tcp
```

```output title="Output" hl_lines="3"
INFO: Found AD domain: inlanefreight.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (inlanefreight.htb:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: dc01.inlanefreight.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Connecting to LDAP server: dc01.inlanefreight.htb
INFO: Found 6 users
INFO: Found 52 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 3 computers
INFO: Found 0 trusts
INFO: Done in 00M 11S
```

## Kerberos Authentication

!!! tip

    Kerberos kimlik doğrulaması Windows için varsayılan kimlik doğrulama yöntemidir. Bu yüzden Kerberos kullanmak ağ trafiğinin daha normal görünmesini sağlar.

Kerberos kimlik doğrulaması kullanılacak ise ad sunucusu kullanmak yeterli olmaz.

Ek olarak Hosts dosyasını da düzenlemek gerekir:

```text title="/etc/hosts" linenums="5"
10.129.136.59 inlanefreight
10.129.136.59 inlanefreight.htb
10.129.136.59 dc01
10.129.136.59 dc01.inlanefreight.htb
```

Şimdi Kerberos kimlik doğrulaması ile bağlantı kuralım:

```sh
my@attack:~$ bloodhound-python -d INLANEFREIGHT.HTB -u 'htb-student' -p 'HTBRocks!' --nameserver 10.129.136.59 -c DCOnly --kerberos
```

```output title="Output"
INFO: Found AD domain: inlanefreight.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc01.inlanefreight.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Connecting to LDAP server: dc01.inlanefreight.htb
INFO: Found 6 users
INFO: Found 52 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 3 computers
INFO: Found 0 trusts
INFO: Done in 00M 11S
```

## Zip Output Files

```sh
my@attack:~$ zip -r inlanefreight.zip *.json
```
