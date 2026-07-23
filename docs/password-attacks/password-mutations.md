# Password Mutations

## Default Rules

```sh
my@attack:~$ ls -l /usr/share/doc/hashcat/rules
```

## Creating a Password File

```sh
my@attack:~$ echo "password" | tee passwords.txt
```

```output title="Output"
password
```

## Creating a Rule File

```text title="custom.rule" linenums="1"
:
c
so0
c so0
sa@
c sa@
c sa@ so0
$!
$! c
$! so0
$! sa@
$! c so0
$! c sa@
$! so0 sa@
$! c so0 sa@
```

## Creating a Wordlist

```sh
my@attack:~$ hashcat passwords.txt -r custom.rule --stdout | tee mutations.txt
```

```output title="Output"
password
Password
passw0rd
Passw0rd
p@ssword
P@ssword
P@ssw0rd
password!
Password!
passw0rd!
p@ssword!
Passw0rd!
P@ssword!
p@ssw0rd!
P@ssw0rd!
```

## Creating a Wordlist with CeWL

```sh
my@attack:~$ cewl https://www.inlanefreight.com -d 4 -m 6 -w inlane.wordlist --lowercase
```
