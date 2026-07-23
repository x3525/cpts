# Virtual Host Fuzzing

## Types

### Name-Based

Tek bir IP adresine ihtiyaç duyulur. Ayırt etmek için HTTP Host başlığı kullanılır.

### IP-Based

Birden fazla IP adresine ihtiyaç duyulur. Ayırt etmek için IP adresi kullanılır.

### Port-Based

Tek bir IP adresine ihtiyaç duyulur. Ayırt etmek için port numarası kullanılır.

## Hosts File

!!! tip

    Sadece ana domain Hosts dosyasına eklendi.

    Her bir wordlist kelimesini el ile eklememek için HTTP başlığı kullanılacak.

```sh
my@attack:~$ echo "94.237.59.180 academy.htb" | sudo tee -a /etc/hosts
```

## Fuzzing

### Ffuf

!!! warning

    URL değeri sabit, ancak HTTP başlığı sabit değil. Bu durumda tüm sonuçlar 200 OK olabilir. Gerçek vHost kelimesi, yanıt boyutu ile ayırt edilebilir.

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u "http://academy.htb:32644" -H "Host: FUZZ.academy.htb"
```

```output title="Output" hl_lines="6 20"
mail                    [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 58ms]
mobile                  [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 60ms]
www                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 64ms]
smtp                    [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 67ms]
webdisk                 [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 67ms]
admin                   [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 68ms]
ns4                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 68ms]
imap                    [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 65ms]
webmail                 [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 68ms]
localhost               [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 68ms]
forum                   [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 68ms]
whm                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 68ms]
autoconfig              [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 70ms]
vpn                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 66ms]
cpanel                  [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 70ms]
ns2                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 70ms]
demo                    [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 66ms]
pop                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 73ms]
ns1                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 69ms]
test                    [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 74ms]
blog                    [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 70ms]
new                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 70ms]
mx                      [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 70ms]
ns                      [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 71ms]
m                       [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 71ms]
dns2                    [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 68ms]
dev                     [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 72ms]
shop                    [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 68ms]
cp                      [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 69ms]
support                 [Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 72ms]
```

### Gobuster

```sh
my@attack:~$ gobuster vhost -w /usr/local/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u "http://academy.htb:32644" --append-domain
```

```output title="Output"
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: test.academy.htb:32644 Status: 200 [Size: 0]
Found: admin.academy.htb:32644 Status: 200 [Size: 0]
```

## Updating Hosts File

```sh
my@attack:~$ echo "94.237.59.180 admin.academy.htb test.academy.htb" | sudo tee -a /etc/hosts
```
