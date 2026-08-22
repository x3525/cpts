# PATH Abuse

## Creating the Script

```sh
htb-student@NIX02:~$ echo 'echo "PATH ABUSE"' > ls
htb-student@NIX02:~$ chmod +x ls
```

## Updating the PATH

```sh
htb-student@NIX02:~$ PATH=$HOME:$PATH
htb-student@NIX02:~$ export PATH
```

## Abusing

```sh
htb-student@NIX02:~$ ls
```

```output title="Output"
PATH ABUSE
```
