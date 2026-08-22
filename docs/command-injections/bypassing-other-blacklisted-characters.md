# Bypassing Other Blacklisted Characters

## Semicolon

```sh
my@attack:~$ tr '!-}' '"-~' <<< ':'
```

## Slash

```sh
my@attack:~$ echo ${PATH:0:1}
```

## Backslash

### CMD

```pwsh
PS C:\Users\my> cmd.exe /c echo %HOMEPATH:~6,-5%
```

### PowerShell

```pwsh
PS C:\Users\my> $env:HOMEPATH[0]
```
