# MySQL

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 3306 | MySQL database system |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.42.195 -p 3306 -sV -sC --script "mysql*"
```

## Dangerous Settings

| OPTION |
|---|
| admin_address |
| debug |
| password |
| secure_file_priv |
| sql_warnings |
| user |

## Service Interaction

```sh
my@attack:~$ mysql -h 10.129.42.195 -u robin -p'robin' --skip-ssl
```
