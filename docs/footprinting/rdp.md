# RDP

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 3389 | Microsoft Terminal Server (RDP) officially registered as Windows Based Terminal (WBT) |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.56.98 -p 3389 -Pn
```

## Service Interaction

### RDesktop

```sh
my@attack:~$ rdesktop 10.129.56.98:3389 -u 'htb-rdp' -p 'HTBRocks!' -d inlanefreight.htb -r disk:linux=/tmp
```

```sh
my@attack:~$ rdesktop 10.129.56.98:3389 -u 'htb-rdp' -p 'HTBRocks!' -a 8 -x m -z -D
```

### xFreeRDP

```sh
my@attack:~$ xfreerdp /v:10.129.56.98:3389 /u:'htb-rdp' /p:'HTBRocks!' /d:inlanefreight.htb /drive:linux,/tmp
```

```sh
my@attack:~$ xfreerdp /v:10.129.56.98:3389 /u:'htb-rdp' /p:'HTBRocks!' /proxy:socks5://127.0.0.1:9050
```

```sh
my@attack:~$ xfreerdp /v:10.129.56.98:3389 /u:'htb-rdp' /p:'HTBRocks!' /sec:rdp
```

```sh
my@attack:~$ xfreerdp /v:10.129.56.98:3389 /u:'htb-rdp' /p:'HTBRocks!' /tls-seclevel:0
```

```sh
my@attack:~$ xfreerdp /v:10.129.56.98:3389 /u:'htb-rdp' /p:'HTBRocks!' +enforce-tlsv1_2
```

```sh
my@attack:~$ xfreerdp /v:10.129.56.98:3389 /u:'htb-rdp' /p:'HTBRocks!' /dynamic-resolution /bpp:8 /network:modem /compression /gdi:hw /gfx:avc444 /rfx +clipboard -themes -wallpaper -fonts -glyph-cache
```

## Brute Forcing Credentials

### Hydra

```sh
my@attack:~$ hydra "rdp://10.129.56.98" -L usernames.txt -P passwords.txt
```

### [Crowbar](https://github.com/galkan/crowbar)

```sh
my@attack:~$ crowbar.py -b rdp -s 10.129.56.98/32 -U usernames.txt -C passwords.txt
```
