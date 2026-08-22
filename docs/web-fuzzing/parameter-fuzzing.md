# Parameter Fuzzing

## Hosts File

```sh
my@attack:~$ echo "94.237.59.180 admin.academy.htb test.academy.htb" | sudo tee -a /etc/hosts
```

## GET

### Ffuf

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u "http://admin.academy.htb:32644/admin/admin.php?FUZZ=value"
```

```output title="Output"
user                    [Status: 200, Size: 783, Words: 221, Lines: 54, Duration: 83ms]
```

### Gobuster

```sh
my@attack:~$ gobuster fuzz -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt -u "http://admin.academy.htb:32644/admin/admin.php?FUZZ=value"
```

```output title="Output"
===============================================================
Starting gobuster in fuzzing mode
===============================================================
Found: [Status=200] [Length=783] [Word=user] http://admin.academy.htb:32644/admin/admin.php?user=value
```

### Wenum

```sh
my@attack:~$ wenum -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt -u "http://admin.academy.htb:32644/admin/admin.php?FUZZ=value"
```

```output title="Output"
 Code    Lines     Words        Size  Method   URL
────────────────────────────────────────────────────────────────────────── Response number 6134: ───────────────────────────────────────────────────────────────────────────
 200      53 L      81 W       783 B  GET      http://admin.academy.htb:32644/admin/admin.php?user=value
```

## POST

!!! warning

    Eğer key=value formatı mevcut ise POST Content-Type değeri application/x-www-form-urlencoded olarak ayarlanır.

| OPTION | DESCRIPTION |
|---|---|
| -X | HTTP yöntemi. |
| -d | POST verisi. |

```sh
my@attack:~$ ffuf -ic -X POST -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u "http://admin.academy.htb:32644/admin/admin.php" -H "Content-Type: application/x-www-form-urlencoded" -d 'FUZZ=value'
```

```output title="Output"
id                      [Status: 200, Size: 768, Words: 219, Lines: 54, Duration: 82ms]
user                    [Status: 200, Size: 783, Words: 221, Lines: 54, Duration: 82ms]
```
