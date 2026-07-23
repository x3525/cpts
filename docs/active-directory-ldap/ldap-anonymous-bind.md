# LDAP Anonymous Bind

## ldapsearch

```sh
my@attack:~$ ldapsearch -H ldap://10.129.42.188 -x -b "" -s base
```

## windapsearch

### Domain [Functional Level](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-functional-levels)

```sh
my@attack:~$ windapsearch.py --dc-ip 10.129.42.188 -d INLANEFREIGHT.LOCAL -u '' --functionality
```

### All Domain Users

```sh
my@attack:~$ windapsearch.py --dc-ip 10.129.42.188 -d INLANEFREIGHT.LOCAL -u '' -U
```

### All Domain Computers

```sh
my@attack:~$ windapsearch.py --dc-ip 10.129.42.188 -d INLANEFREIGHT.LOCAL -u '' -C
```
