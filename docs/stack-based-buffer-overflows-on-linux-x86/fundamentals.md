# Fundamentals

!!! warning

    64-bit mimari için:

    * [Link](https://www.ired.team/offensive-security/code-injection-process-injection/binary-exploitation/64-bit-stack-based-buffer-overflow)
    * [Link](https://blog.techorganic.com/2015/04/10/64-bit-linux-stack-smashing-tutorial-part-1/)

## Vulnerable Code

```c title="bow.c" linenums="1" hl_lines="8"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int bowfunc(char *string) {

    char buffer[1024];
    strcpy(buffer, string);
    return 1;
}

int main(int argc, char *argv[]) {

    bowfunc(argv[1]);
    printf("Done.\n");
    return 1;
}
```

## Compilation

[ASLR](https://en.wikipedia.org/wiki/Address_space_layout_randomization) bellek korumasını devre dışı bırakalım:

```sh
my@attack:~$ echo 0 | sudo tee /proc/sys/kernel/randomize_va_space
```

C kodunu 32-bit için derleyelim:

```sh
my@attack:~$ gcc -m32 -o bow32 bow.c -z execstack -fno-stack-protector
```

## Stack RWE Executable

```sh
my@attack:~$ readelf -l bow32 | grep STACK
```

```output title="Output"
GNU_STACK      0x000000 0x00000000 0x00000000 0x00000 0x00000 RWE 0x10
```

## Stack Protection

```sh
my@attack:~$ readelf -s bow32 | grep stack_chk
```

## AT&T Syntax

| MEMORY ADDRESS | ADDRESS JUMPS | ASSEMBLER INSTRUCTION | OPERATION SUFFIXES |
|---|---|---|---|
| 0x000011cd | <+11> | mov | %esp,%ebp |

```sh
my@attack:~$ gdb -q bow32
(gdb) disassemble main
```

```nasm title="Output" hl_lines="6"
Dump of assembler code for function main:
   0x000011c2 <+0>:     lea    0x4(%esp),%ecx
   0x000011c6 <+4>:     and    $0xfffffff0,%esp
   0x000011c9 <+7>:     push   -0x4(%ecx)
   0x000011cc <+10>:    push   %ebp
   0x000011cd <+11>:    mov    %esp,%ebp
   0x000011cf <+13>:    push   %ebx
   0x000011d0 <+14>:    push   %ecx
   0x000011d1 <+15>:    call   0x1090 <__x86.get_pc_thunk.bx>
   0x000011d6 <+20>:    add    $0x2e1e,%ebx
   0x000011dc <+26>:    mov    %ecx,%eax
   0x000011de <+28>:    mov    0x4(%eax),%eax
   0x000011e1 <+31>:    add    $0x4,%eax
   0x000011e4 <+34>:    mov    (%eax),%eax
   0x000011e6 <+36>:    sub    $0xc,%esp
   0x000011e9 <+39>:    push   %eax
   0x000011ea <+40>:    call   0x118d <bowfunc>
   0x000011ef <+45>:    add    $0x10,%esp
   0x000011f2 <+48>:    sub    $0xc,%esp
   0x000011f5 <+51>:    lea    -0x1fec(%ebx),%eax
   0x000011fb <+57>:    push   %eax
   0x000011fc <+58>:    call   0x1050 <puts@plt>
   0x00001201 <+63>:    add    $0x10,%esp
   0x00001204 <+66>:    mov    $0x1,%eax
   0x00001209 <+71>:    lea    -0x8(%ebp),%esp
   0x0000120c <+74>:    pop    %ecx
   0x0000120d <+75>:    pop    %ebx
   0x0000120e <+76>:    pop    %ebp
   0x0000120f <+77>:    lea    -0x4(%ecx),%esp
   0x00001212 <+80>:    ret
End of assembler dump.
```

## Change the Syntax to Intel

Intel formatına geçmek için GDB içerisinde aşağıda verilen komutu çalıştır:

```sh
(gdb) set disassembly-flavor intel
```

Değişimi kalıcı hale getirmek için aşağıdaki komutu çalıştır:

```sh
my@attack:~$ echo "set disassembly-flavor intel" > ~/.gdbinit
```

## Intel Syntax

| MEMORY ADDRESS | ADDRESS JUMPS | ASSEMBLER INSTRUCTION | OPERATION SUFFIXES |
|---|---|---|---|
| 0x000011cd | <+11> | mov | ebp,esp |

```sh
my@attack:~$ gdb -q bow32
(gdb) disassemble main
```

```nasm title="Output" hl_lines="6"
Dump of assembler code for function main:
   0x000011c2 <+0>:     lea    ecx,[esp+0x4]
   0x000011c6 <+4>:     and    esp,0xfffffff0
   0x000011c9 <+7>:     push   DWORD PTR [ecx-0x4]
   0x000011cc <+10>:    push   ebp
   0x000011cd <+11>:    mov    ebp,esp
   0x000011cf <+13>:    push   ebx
   0x000011d0 <+14>:    push   ecx
   0x000011d1 <+15>:    call   0x1090 <__x86.get_pc_thunk.bx>
   0x000011d6 <+20>:    add    ebx,0x2e1e
   0x000011dc <+26>:    mov    eax,ecx
   0x000011de <+28>:    mov    eax,DWORD PTR [eax+0x4]
   0x000011e1 <+31>:    add    eax,0x4
   0x000011e4 <+34>:    mov    eax,DWORD PTR [eax]
   0x000011e6 <+36>:    sub    esp,0xc
   0x000011e9 <+39>:    push   eax
   0x000011ea <+40>:    call   0x118d <bowfunc>
   0x000011ef <+45>:    add    esp,0x10
   0x000011f2 <+48>:    sub    esp,0xc
   0x000011f5 <+51>:    lea    eax,[ebx-0x1fec]
   0x000011fb <+57>:    push   eax
   0x000011fc <+58>:    call   0x1050 <puts@plt>
   0x00001201 <+63>:    add    esp,0x10
   0x00001204 <+66>:    mov    eax,0x1
   0x00001209 <+71>:    lea    esp,[ebp-0x8]
   0x0000120c <+74>:    pop    ecx
   0x0000120d <+75>:    pop    ebx
   0x0000120e <+76>:    pop    ebp
   0x0000120f <+77>:    lea    esp,[ecx-0x4]
   0x00001212 <+80>:    ret
End of assembler dump.
```

## Presentation of the Instructions

### AT&T

| ASSEMBLER INSTRUCTION | SOURCE | DESTINATION |
|---|---|---|
| mov | %esp | %ebp |

### Intel

| ASSEMBLER INSTRUCTION | DESTINATION | SOURCE |
|---|---|---|
| mov | ebp | esp |
