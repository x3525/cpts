# SMTP

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 25 | Simple Mail Transfer Protocol (SMTP), used for email routing between mail servers |
| 465 | Message Submission over TLS protocol |
| 587 | Email Message Submission (No longer preferred; see port 465.) |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.42.195 -p 25,465,587 -sV -sC --script smtp-open-relay
```

## Dangerous Settings

| OPTION | VALUE |
|---|---|
| mynetworks | 0.0.0.0/0 |

## Service Interaction

```sh
my@attack:~$ telnet 10.129.42.195 25
VRFY thatuserdoesnotexist
```

```output title="Output"
550 5.1.1 <thatuserdoesnotexist>: Recipient address rejected: User unknown in local recipient table
```

## Valid Usernames

### [smtp-user-enum](https://github.com/cytopia/smtp-user-enum) (Python)

```sh
my@attack:~$ smtp-user-enum 10.129.42.195 25 -d inlanefreight.htb -U usernames.txt -m RCPT
```

### [smtp-user-enum](https://github.com/pentestmonkey/smtp-user-enum)

```sh
my@attack:~$ smtp-user-enum -t 10.129.42.195 -p 25 -D inlanefreight.htb -U usernames.txt -M RCPT
```

### [o365spray](https://github.com/0xZDH/o365spray)

#### Validate Office 365

```sh
my@attack:~$ o365spray.py --domain msplaintext.xyz --validate
```

#### Enumeration

```sh
my@attack:~$ o365spray.py --domain msplaintext.xyz -U usernames.txt --enum
```

#### Password Spraying

```sh
my@attack:~$ o365spray.py --domain msplaintext.xyz -U usernames.txt -p 'March2022!' --spray --count 1 --lockout 1
```
