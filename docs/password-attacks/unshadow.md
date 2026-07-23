# Unshadow

## Shadow File

### File Format

| USERNAME | PASSWORD HASH | LAST PASSWORD CHANGE | MINIMUM PASSWORD AGE | MAXIMUM PASSWORD AGE | WARNING PERIOD | INACTIVITY PERIOD | EXPIRATION DATE | RESERVED |
|---|---|---|---|---|---|---|---|---|
| root | $y$j9T$EWwQqzatkxHBAHuRsgz1m1$n9Re3Vru0rLpJGCWq9p.IG2VXiKhe3TiGdILS/y00l. | 20077 | 0 | 99999 | 7 ||||

### Encrypted Password Field

| PREFIX | PARAMETER | SALT | HASH |
|---|---|---|---|
| $y$ | j9T | EWwQqzatkxHBAHuRsgz1m1 | n9Re3Vru0rLpJGCWq9p.IG2VXiKhe3TiGdILS/y00l. |

### Algorithms

| PREFIX | TYPE |
|---|---|
| $1$ | [md5crypt](https://man.archlinux.org/man/crypt.5.en#md5crypt) |
| $2a$ | [bcrypt](https://man.archlinux.org/man/crypt.5.en#bcrypt) |
| $2b$ | [bcrypt](https://man.archlinux.org/man/crypt.5.en#bcrypt) |
| $2x$ | [bcrypt](https://man.archlinux.org/man/crypt.5.en#bcrypt) |
| $2y$ | [bcrypt](https://man.archlinux.org/man/crypt.5.en#bcrypt) |
| $3$ | [NT](https://man.archlinux.org/man/crypt.5.en#NT) |
| $5$ | [sha256crypt](https://man.archlinux.org/man/crypt.5.en#sha256crypt) |
| $6$ | [sha512crypt](https://man.archlinux.org/man/crypt.5.en#sha512crypt) |
| $7$ | [scrypt](https://man.archlinux.org/man/crypt.5.en#scrypt) |
| $gy$ | [gost-yescrypt](https://man.archlinux.org/man/crypt.5.en#gost-yescrypt) |
| $sha1$ | [sha1crypt](https://man.archlinux.org/man/crypt.5.en#sha1crypt) |
| $y$ | [yescrypt](https://man.archlinux.org/man/crypt.5.en#yescrypt) |

## Cracking Hashes

### Backup Passwd File

```sh
root@nix01:~# cp /etc/passwd passwd.bak
```

### Backup Shadow File

```sh
root@nix01:~# cp /etc/shadow shadow.bak
```

### John the Ripper

```sh
my@attack:~$ unshadow passwd.bak shadow.bak > unshadow.hash
```

### Hashcat

```sh
my@attack:~$ hashcat --hwmon-disable -a 0 -m 1800 unshadow.hash /usr/local/share/wordlists/rockyou.txt
```
