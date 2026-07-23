# Evasion Tools

## [Bashfuscator](https://github.com/Bashfuscator/Bashfuscator)

```sh
my@attack:~$ git clone https://github.com/Bashfuscator/Bashfuscator.git
my@attack:~$ cd Bashfuscator
my@attack:~/Bashfuscator$ pip3 install setuptools argcomplete pyperclip
my@attack:~/Bashfuscator$ python3 setup.py install --user
my@attack:~/Bashfuscator$ mv bashfuscator/bin/bashfuscator Bashfuscator
my@attack:~/Bashfuscator$ python3 Bashfuscator -c 'cat /etc/hosts' -s 1 -t 1 --layers 1 --no-mangling
```

```output title="Output" hl_lines="4"
[+] Mutators used: Token/ForCode
[+] Payload:

$BASH <<< "$(Zu=(\  o t a e \/ c s h);for QL in 6 3 2 0 5 4 2 6 5 8 1 7 2 7;do printf %s "${Zu[$QL]}";done;)"

[+] Payload size: 109 characters
```

## [Invoke-DOSfuscation](https://github.com/danielbohannon/Invoke-DOSfuscation)

```pwsh
PS C:\Users\my> git clone https://github.com/danielbohannon/Invoke-DOSfuscation.git
PS C:\Users\my> cd Invoke-DOSfuscation
PS C:\Users\my\Invoke-DOSfuscation> Import-Module .\Invoke-DOSfuscation.psd1
PS C:\Users\my\Invoke-DOSfuscation> Invoke-DOSfuscation
Invoke-DOSfuscation> SET COMMAND type C:\Windows\System32\drivers\etc\hosts
Invoke-DOSfuscation> ENCODING
Invoke-DOSfuscation\Encoding> 1
```

```output title="Output" hl_lines="6"
Executed:
  CLI:  Encoding\1
  FULL: Out-EnvVarEncodedCommand -StringToEncode $Command -ObfuscationLevel 1 -MaintainCase

Result:
ty%APPDATA:~-13,-12%%TMP:~-3,1% C:\W%CommonProgramFiles(x86):~-23,1%ndows\Syst%CommonProgramFiles(x86):~14,1%m32\drivers\etc\hosts
```
