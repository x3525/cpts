# Email Services

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 110 | Post Office Protocol, version 3 (POP3) |
| 143 | Internet Message Access Protocol (IMAP), management of electronic mail messages on a server |
| 993 | Internet Message Access Protocol over TLS/SSL (IMAPS) |
| 995 | Post Office Protocol 3 over TLS/SSL (POP3S) |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.97.10 -p 110,143,993,995 -sV -sC
```

## Service Interaction

### cURL

```sh
my@attack:~$ curl -s 'imaps://10.129.97.10' --user robin:robin --insecure -v
```

### OpenSSL

```sh
my@attack:~$ openssl s_client -connect 10.129.97.10:imaps
```

### Telnet

```sh
my@attack:~$ telnet 10.129.97.10 110
USER thatuserdoesnotexist
```

```output title="Output"
-ERR [AUTH] Plaintext authentication disallowed on non-secure (SSL/TLS) connections.
```
