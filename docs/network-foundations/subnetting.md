# Subnetting

## Example

| IPV4 ADDRESS | SUBNET MASK | CIDR |
|---|---|---|
| 192.168.12.160 | 255.255.255.192 | 192.168.12.160/26 |

## Network Part

| 1ST OCTET | 2ND OCTET | 3RD OCTET | 4TH OCTET | DECIMAL |
|---|---|---|---|---|
| <span style="color:orange">11000000</span> | <span style="color:orange">10101000</span> | <span style="color:orange">00001100</span> | <span style="color:orange">10</span>100000 | 192.168.12.160/<span style="color:orange">26</span> |
| <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11</span>000000 | <span style="color:orange">255.255.255.192</span> |

## Host Part

| 1ST OCTET | 2ND OCTET | 3RD OCTET | 4TH OCTET | DECIMAL |
|---|---|---|---|---|
| 11000000 | 10101000 | 00001100 | 10<span style="color:orange">100000</span> | 192.168.12.160/26 |
| 11111111 | 11111111 | 11111111 | 11<span style="color:orange">000000</span> | 255.255.255.192 |

## Separation

!!! abstract

    Network ve host ayrımı belirlenir.

| 1ST OCTET | 2ND OCTET | 3RD OCTET | 4TH OCTET | DECIMAL |
|---|---|---|---|---|
| 11000000 | 10101000 | 00001100 | 10 100000 | 192.168.12.160/26 |
| <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11</span> 000000 | 255.255.255.192 |

## Network Address

!!! abstract

    Host bitleri 0 yapılır.

| 1ST OCTET | 2ND OCTET | 3RD OCTET | 4TH OCTET | DECIMAL |
|---|---|---|---|---|
| 11000000 | 10101000 | 00001100 | 10 <span style="color:orange">000000</span> | <span style="color:orange">192.168.12.128</span>/26 |
| <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11</span> 000000 | 255.255.255.192 |

## Broadcast Address

!!! abstract

    Host bitleri 1 yapılır.

| 1ST OCTET | 2ND OCTET | 3RD OCTET | 4TH OCTET | DECIMAL |
|---|---|---|---|---|
| 11000000 | 10101000 | 00001100 | 10 <span style="color:orange">111111</span> | <span style="color:orange">192.168.12.191</span>/26 |
| <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11</span> 000000 | 255.255.255.192 |

## Subnetting Into Smaller Networks

Mevcut ağı 4 adet alt ağa bölelim:

* 2^H = 4
* Alt ağ maskesinin host kısmına eklenecek 1 adedi H adet olmalıdır.

| 1ST OCTET | 2ND OCTET | 3RD OCTET | 4TH OCTET | DECIMAL |
|---|---|---|---|---|
| 11000000 | 10101000 | 00001100 | 1000 0000 | 192.168.12.128/<span style="color:orange">28</span> |
| <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | <span style="color:orange">11111111</span> | 11<span style="color:orange">11</span> 0000 | <span style="color:orange">255.255.255.240</span> |

* Alt ağ maskesinin host kısmına H adet 1 ekledikten sonra geriye 4 adet 0 kalır.
* 2^4 = A
* Her bir alt ağ için A adet IP adresi mevcuttur.

| SUBNET NUMBER | NETWORK ADDRESS | FIRST HOST | LAST ADDRESS | BROADCAST ADDRESS |
|---|---|---|---|---|
| 1 | <span style="color:orange">192.168.12.128</span> | 192.168.12.129 | 192.168.12.142 | <span style="color:orange">192.168.12.143</span> |
| 2 | <span style="color:orange">192.168.12.144</span> | 192.168.12.145 | 192.168.12.158 | <span style="color:orange">192.168.12.159</span> |
| 3 | <span style="color:orange">192.168.12.160</span> | 192.168.12.161 | 192.168.12.174 | <span style="color:orange">192.168.12.175</span> |
| 4 | <span style="color:orange">192.168.12.176</span> | 192.168.12.177 | 192.168.12.190 | <span style="color:orange">192.168.12.191</span> |

## Record Route Field

```sh
my@attack:~$ ping 10.129.143.158 -c 1 -R
```

```output title="Output"
PING 10.129.143.158 (10.129.143.158) 56(124) bytes of data.
64 bytes from 10.129.143.158: icmp_seq=1 ttl=63 time=11.7 ms
RR:     10.10.14.38
        10.129.0.1
        10.129.143.158
        10.129.143.158
        10.10.14.1
        10.10.14.38

--- 10.129.143.158 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 11.688/11.688/11.688/0.000 ms
```
