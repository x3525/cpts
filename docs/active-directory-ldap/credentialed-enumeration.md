# Credentialed Enumeration

## ldapsearch-ad

### Password Policy

```sh
my@attack:~$ ldapsearch-ad.py -l 10.129.42.188 -d INLANEFREIGHT.LOCAL -u 'james.cross' -p 'Academy_Student!' -t pass-pols
```

### Subject to AS-REP Roasting

```sh
my@attack:~$ ldapsearch-ad.py -l 10.129.42.188 -d INLANEFREIGHT.LOCAL -u 'james.cross' -p 'Academy_Student!' -t asreproast
```

### Subject to Kerberoasting

```sh
my@attack:~$ ldapsearch-ad.py -l 10.129.42.188 -d INLANEFREIGHT.LOCAL -u 'james.cross' -p 'Academy_Student!' -t kerberoast
```

## windapsearch

### Domain Administrators

```sh
my@attack:~$ windapsearch.py --dc-ip 10.129.42.188 -d INLANEFREIGHT.LOCAL -u INLANEFREIGHT\\'james.cross' --da
```

### Unconstrained Delegation

```sh
my@attack:~$ windapsearch.py --dc-ip 10.129.42.188 -d INLANEFREIGHT.LOCAL -u INLANEFREIGHT\\'james.cross' --unconstrained-users
```
