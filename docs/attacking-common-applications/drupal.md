# Drupal

## Version

```sh
my@attack:~$ curl -s 'http://drupal-acc.inlanefreight.local' | grep Drupal
```

```html title="Output"
<meta name="Generator" content="Drupal 7 (http://drupal.org)" />
```

## CHANGELOG.txt

```sh
my@attack:~$ curl -s 'http://drupal-acc.inlanefreight.local/CHANGELOG.txt' | head
```

```output title="Output" hl_lines="1"
Drupal 7.57, 2018-02-21
-----------------------
- Fixed security issues (multiple vulnerabilities). See SA-CORE-2018-001.

Drupal 7.56, 2017-06-21
-----------------------
- Fixed security issues (access bypass). See SA-CORE-2017-003.

Drupal 7.55, 2017-06-07
```

## Nodes

```sh
my@attack:~$ curl -s 'http://drupal-acc.inlanefreight.local/node/1' -I
```

```http title="Output" hl_lines="9"
HTTP/1.1 200 OK
Date: Tue, 06 May 2025 21:50:37 GMT
Server: Apache/2.4.41 (Ubuntu)
Expires: Sun, 19 Nov 1978 05:00:00 GMT
Cache-Control: no-cache, must-revalidate
X-Content-Type-Options: nosniff
Content-Language: en
X-Frame-Options: SAMEORIGIN
X-Generator: Drupal 7 (http://drupal.org)
Link: </node/1>; rel="canonical",</node/1>; rel="shortlink"
Content-Type: text/html; charset=utf-8
```

## [droopescan](https://github.com/SamJoan/droopescan)

```sh
my@attack:~$ droopescan scan drupal -u "http://drupal-acc.inlanefreight.local" -e a
```

```output title="Output"
[+] Plugins found:
    profile http://drupal-acc.inlanefreight.local/modules/profile/
    php http://drupal-acc.inlanefreight.local/modules/php/
    image http://drupal-acc.inlanefreight.local/modules/image/

[+] Themes found:
    seven http://drupal-acc.inlanefreight.local/themes/seven/
    garland http://drupal-acc.inlanefreight.local/themes/garland/

[+] Possible version(s):
    7.57

[+] Possible interesting urls found:
    Default changelog file - http://drupal-acc.inlanefreight.local/CHANGELOG.txt
    Default admin - http://drupal-acc.inlanefreight.local/user/login

[+] Scan finished (0:04:28.440604 elapsed)
```
