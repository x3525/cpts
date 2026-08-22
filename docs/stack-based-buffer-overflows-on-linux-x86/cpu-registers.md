# CPU Registers

## Data Registers

| 32-BIT | 64-BIT | MEANING | DESCRIPTION |
|---|---|---|---|
| EAX | RAX | Accumulator | Aritmetik ve I/O işlemlerinde kullanılır. |
| EBX | RBX | Base | Adresleme işlemlerinde kullanılır. |
| ECX | RCX | Counter | Döngüleri saymak için kullanılır. |
| EDX | RDX | Data | Büyük değerleri içeren çarpma ve bölme işlemlerinde kullanılır. |

## Pointer Registers

| 32-BIT | 64-BIT | MEANING | DESCRIPTION |
|---|---|---|---|
| EIP | RIP | Instruction Pointer | Bir sonraki talimatın offset adresini tutar. |
| ESP | RSP | Stack Pointer | Yığın tepesini işaret eder. |
| EBP | RBP | Base/Frame Pointer | Yığın tabanını işaret eder. |

## Stack Frames

### Prologue

| ACTION | DESCRIPTION |
|---|---|
| push ebp | EBP yığına push edilir. Böylece EBP yedeklenir. |
| mov ebp,esp | EBP değeri ESP değeri ile güncellenir. Böylece EBP yığın tepesini işaret eder. |
| sub esp,0x404 | ESP değeri azaltılarak gerekli işlemler için yığında yer açılır. |

```sh
(gdb) disassemble bowfunc
```

```nasm title="Output" hl_lines="2-4"
Dump of assembler code for function bowfunc:
   0x0000118d <+0>:     push   ebp
   0x0000118e <+1>:     mov    ebp,esp
   0x00001191 <+4>:     sub    esp,0x404
   0x00001197 <+10>:    call   0x1213 <__x86.get_pc_thunk.ax>
   0x0000119c <+15>:    add    eax,0x2e58
   0x000011a1 <+20>:    sub    esp,0x8
   0x000011a4 <+23>:    push   DWORD PTR [ebp+0x8]
   0x000011a7 <+26>:    lea    edx,[ebp-0x408]
   0x000011ad <+32>:    push   edx
   0x000011ae <+33>:    mov    ebx,eax
   0x000011b0 <+35>:    call   0x1040 <strcpy@plt>
   0x000011b5 <+40>:    add    esp,0x10
   0x000011b8 <+43>:    mov    eax,0x1
   0x000011bd <+48>:    mov    ebx,DWORD PTR [ebp-0x4]
   0x000011c0 <+51>:    mov esp,ebp
   0x000011c1 <+52>:    pop ebp
   0x000011c2 <+53>:    ret
End of assembler dump.
```

### Epilogue

| ACTION | DESCRIPTION |
|---|---|
| mov esp,ebp | ESP değeri EBP değeri ile güncellenir. Böylece daha önceden tahsis edilen yer serbest bırakılır. |
| pop ebp | EBP yığından pop edilir. Böylece yedeklenen EBP geri yüklenir. |
| ret | Çağıran fonksiyona geri dönülür. |

```sh
(gdb) disassemble bowfunc
```

```nasm title="Output" hl_lines="16-18"
Dump of assembler code for function bowfunc:
   0x0000118d <+0>:     push   ebp
   0x0000118e <+1>:     mov    ebp,esp
   0x00001191 <+4>:     sub    esp,0x404
   0x00001197 <+10>:    call   0x1213 <__x86.get_pc_thunk.ax>
   0x0000119c <+15>:    add    eax,0x2e58
   0x000011a1 <+20>:    sub    esp,0x8
   0x000011a4 <+23>:    push   DWORD PTR [ebp+0x8]
   0x000011a7 <+26>:    lea    edx,[ebp-0x408]
   0x000011ad <+32>:    push   edx
   0x000011ae <+33>:    mov    ebx,eax
   0x000011b0 <+35>:    call   0x1040 <strcpy@plt>
   0x000011b5 <+40>:    add    esp,0x10
   0x000011b8 <+43>:    mov    eax,0x1
   0x000011bd <+48>:    mov    ebx,DWORD PTR [ebp-0x4]
   0x000011c0 <+51>:    mov esp,ebp
   0x000011c1 <+52>:    pop ebp
   0x000011c2 <+53>:    ret
End of assembler dump.
```

## Endianness

### Checking for System

=== "LINUX"

    ```sh
    my@attack:~$ LANG=C lscpu | grep 'Byte Order'
    ```

    ```output title="Output"
    Byte Order:                           Little Endian
    ```

=== "PYTHON"

    ```sh
    my@attack:~$ python3 -c 'import sys; print(sys.byteorder)'
    ```

    ```output title="Output"
    little
    ```

### Big

| 0XFFFF0000 | 0XFFFF0001 | 0XFFFF0002 | 0XFFFF0003 |
|---|---|---|---|
| de | ad | be | ef |

### Little

| 0XFFFF0000 | 0XFFFF0001 | 0XFFFF0002 | 0XFFFF0003 |
|---|---|---|---|
| ef | be | ad | de |
