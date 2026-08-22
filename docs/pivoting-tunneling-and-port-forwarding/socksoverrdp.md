# SocksOverRDP

## Necessary Files

* [SocksOverRDP-x64.zip](https://github.com/nccgroup/SocksOverRDP/releases/download/v1.0/SocksOverRDP-x64.zip)
* [ProxifierPE.zip](https://www.proxifier.com/download/ProxifierPE.zip)

## RDP 1

### Loading the DLL

```pwsh
PS C:\Users\htb-student\Desktop\SocksOverRDP-x64> regsvr32 SocksOverRDP-Plugin.dll
```

```output title="Output"
DllRegisterServer in SocksOverRDP-Plugin.dll succeeded.
```

### [mstsc](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/mstsc)

```pwsh
PS C:\Users\htb-student> C:\Windows\System32\mstsc.exe
```

1. Show Options
2. Logon settings
    * Computer: 172.16.5.19
    * User name: victor
    * Password: pass@123
3. Local Resources
    * More
    * Drives: Local Disk (C:)

## RDP 2

### Copying the Server Binary

```pwsh
PS C:\Users\victor> cmd.exe /c copy \\tsclient\C\Users\htb-student\Desktop\SocksOverRDP-x64\SocksOverRDP-Server.exe C:\Users\victor\Desktop
```

### Executing the Binary

```pwsh
PS C:\Users\victor> C:\Users\victor\Desktop\SocksOverRDP-Server.exe
```

```output title="Output"
Socks Over RDP by Balazs Bucsay [[@xoreipeip]]

[*] Channel opened over RDP
```

## RDP 1 for RDP 3

### Confirming Listener

```pwsh
PS C:\Users\htb-student> netstat -antb | findstr /C:1080
```

```output title="Output"
TCP    127.0.0.1:1080         0.0.0.0:0              LISTENING
```

### Proxifier Profile Creation

1. Profile
2. Proxy Servers
3. Add
4. Server
    * Address: 127.0.0.1
    * Port: 1080
    * Protocol: SOCKS Version 5

## RDP 3

```pwsh
PS C:\Users\htb-student> C:\Windows\System32\mstsc.exe
```

1. Show Options
2. Logon settings
    * Computer: 172.16.6.155
    * User name: jason
    * Password: WellConnected123!
