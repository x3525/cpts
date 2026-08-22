# Targets

## Show Targets

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > show targets
```

```output title="Output"
Exploit targets:
=================

    Id  Name
    --  ----
=>  0   Automatic
    1   PowerShell
    2   Native upload
    3   MOF upload
```

## Target Selection

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > set TARGET 1
```
