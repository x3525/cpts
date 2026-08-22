# Protected File Transfers

## Encryption on Windows

[Invoke-AESEncryption.ps1](https://www.powershellgallery.com/packages/DRTools/4.0.3.4/Content/Functions%5CInvoke-AESEncryption.ps1) dosyasını içeren [DRTools](https://www.powershellgallery.com/packages/DRTools/4.0.3.4) modülünü yükle:

```pwsh
PS C:\Users\htb-student> Install-Module DRTools
```

Transfer edilecek bir dosyayı şifrele:

```pwsh
PS C:\Users\htb-student> Invoke-AESEncryption -Mode Encrypt -Key "p4ssw0rd" -Path "file.txt"
```

Dosyanın şifresini çöz:

```pwsh
PS C:\Users\my> Invoke-AESEncryption -Mode Decrypt -Key "p4ssw0rd" -Path "file.txt.aes"
```

## Encryption on Linux

Transfer edilecek bir dosyayı şifrele:

```sh
htb-student@nix04:~$ openssl enc -aes256 -iter 100000 -pbkdf2 -in /etc/passwd -out passwd.enc
```

Dosyanın şifresini çöz:

```sh
my@attack:~$ openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd
```
