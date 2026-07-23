# IP Addresses

## IPv4 Structure

| NOTATION | PRESENTATION |
|---|---|
| Binary | 01111111.00000000.00000000.00000001 |
| Decimal | 127.0.0.1 |

## Subnet Mask

| CLASS | FIRST ADDRESS | LAST ADDRESS | SUBNET MASK |
|---|---|---|---|
| A | 1.0.0.1 | 127.255.255.255 | <span style="color:orange">255.0.0.0</span> |
| B | 128.0.0.1 | 191.255.255.255 | <span style="color:orange">255.255.0.0</span> |
| C | 192.0.0.1 | 223.255.255.255 | <span style="color:orange">255.255.255.0</span> |
| D | 224.0.0.1 | 239.255.255.255 | <span style="color:orange">Multicast</span> |
| E | 240.0.0.1 | 255.255.255.255 | <span style="color:orange">Reserved</span> |

## Network and Gateway Addresses

| CLASS | FIRST ADDRESS | LAST ADDRESS | SUBNET MASK |
|---|---|---|---|
| A | <span style="color:orange">1.0.0.1</span> | 127.255.255.255 | 255.0.0.0 |
| B | <span style="color:orange">128.0.0.1</span> | 191.255.255.255 | 255.255.0.0 |
| C | <span style="color:orange">192.0.0.1</span> | 223.255.255.255 | 255.255.255.0 |
| D | <span style="color:orange">224.0.0.1</span> | 239.255.255.255 | Multicast |
| E | <span style="color:orange">240.0.0.1</span> | 255.255.255.255 | Reserved |

## Broadcast Address

| CLASS | FIRST ADDRESS | LAST ADDRESS | SUBNET MASK |
|---|---|---|---|
| A | 1.0.0.1 | <span style="color:orange">127.255.255.255</span> | 255.0.0.0 |
| B | 128.0.0.1 | <span style="color:orange">191.255.255.255</span> | 255.255.0.0 |
| C | 192.0.0.1 | <span style="color:orange">223.255.255.255</span> | 255.255.255.0 |
| D | 224.0.0.1 | <span style="color:orange">239.255.255.255</span> | Multicast |
| E | 240.0.0.1 | <span style="color:orange">255.255.255.255</span> | Reserved |
