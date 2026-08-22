# Fuzzing Parameters

## Attach

Free CD to MP3 Converter programını başlat.

Programı [x32dbg](https://github.com/x64dbg/x64dbg) üzerinde açmak için aşağıda verilen yolu takip et:

* File
* Attach
* C:\Program Files\CD to MP3 Freeware\cdextract.exe
* Attach

## Skip Breaking Breakpoints

Aşağıda verilen yolu takip et:

* Options
* Preferences
* Events

Tüm işaretleri kaldır:

* [ ] Debug Settings
* [ ] DLL Entry
* [ ] DLL Load
* [ ] DLL Unload
* [ ] Entry Breakpoint*
* [ ] Exit Breakpoint*
* [ ] System Breakpoint*
* [ ] System DLL Entry
* [ ] System DLL Load
* [ ] System DLL Unload
* [ ] System TLS Callbacks*
* [ ] Thread Entry
* [ ] Thread Start
* [ ] Threat End
* [ ] TLS Callbacks*

## Fuzzing Text Fields

Öncelikle 10000 adet A karakteri (0x41) içeren bir dizi oluştur:

```pwsh
PS C:\Users\htb-student> python -c 'print("A" * 10000)'
```

Elde edilen değeri aşağıda verilen alanlar üzerinde test et:

* Powered by
* Registration Code

Program crash vermedi:

```text title="Free CD to MP3 Converter"
Registration is not valid, please try again.
```

## Fuzzing Opened File

Öncelikle 10000 adet A karakteri (0x41) içeren bir ses dosyası oluştur:

```pwsh
PS C:\Users\htb-student> python -c 'print("A" * 10000, file=open("fuzz.wav", "w"))'
```

Ses dosyasını Free CD to MP3 Converter üzerinde açmak için aşağıda verilen yolu takip et:

* Encode
* C:\Users\htb-student\fuzz.wav
* Open

Program crash verdi:

```text title="Paused"
First chance exception 41414141 (C0000005, EXCEPTION_ACCESS_VIOLATION)!
```

EIP register değerini görüntüleyelim (41414141, AAAA):

```nasm title="Registers" hl_lines="9"
EAX : 00000000
EBX : 41414141
ECX : 000016A0
EDX : 00002712
EBP : 41414141
ESP : 0014F974     "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
ESI : 41414141
EDI : 41414141
EIP : 41414141
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
LastError : 000003E6 (ERROR_NOACCESS)
LastStatus : C0000005 (STATUS_ACCESS_VIOLATION)
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
