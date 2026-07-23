# Filtering

## Hosts File

```sh
my@attack:~$ echo "94.237.59.180 academy.htb" | sudo tee -a /etc/hosts
```

## Finding Filter Size

=== "CONTENT-LENGTH"

    ```sh
    my@attack:~$ curl -s 'http://academy.htb:32644' -I -H 'Host: thatpagedoesnotexist.academy.htb' | grep -i Content-Length:
    ```

    ```output title="Output"
    Content-Length: 986
    ```

=== "BYTE COUNT"

    ```sh
    my@attack:~$ curl -s 'http://academy.htb:32644' -H 'Host: thatpagedoesnotexist.academy.htb' | wc -c
    ```

    ```output title="Output"
    986
    ```

## Fuzzing

### Ffuf

| OPTION | DESCRIPTION |
|---|---|
| -H | HTTP başlığı. |
| -fs | Yanıt boyutu filtresi. |

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/common.txt:FUZZ -u "http://academy.htb:32644" -H "Host: FUZZ.academy.htb" -fs 986
```

```output title="Output"
ADMIN                   [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 64ms]
Admin                   [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 65ms]
admin                   [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 64ms]
test                    [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 73ms]
```

### Gobuster

| OPTION | DESCRIPTION |
|---|---|
| --append-domain | Ana domain her bir wordlist kelimesine eklenir. |
| --exclude-length | Yanıt boyutu filtresi. |

```sh
my@attack:~$ gobuster vhost -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u "http://academy.htb:32644" --append-domain --exclude-length 986
```

```output title="Output"
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: ADMIN.academy.htb:32644 Status: 200 [Size: 0]
Found: Admin.academy.htb:32644 Status: 200 [Size: 0]
Found: admin.academy.htb:32644 Status: 200 [Size: 0]
Found: test.academy.htb:32644 Status: 200 [Size: 0]
```

## Updating Hosts File

```sh
my@attack:~$ echo "94.237.59.180 admin.academy.htb test.academy.htb" | sudo tee -a /etc/hosts
```
