# Enumerating Security Controls

## Anti-Malware Status

```pwsh
PS C:\Users\htb-student> Get-MpComputerStatus
```

## AppLocker

```pwsh
PS C:\Users\htb-student> Get-AppLockerPolicy -Effective | Select-Object -ExpandProperty RuleCollections
```

## PowerShell Language Mode

```pwsh
PS C:\Users\htb-student> $ExecutionContext.SessionState.LanguageMode
```

## [LAPSToolkit](https://github.com/leoloobeek/LAPSToolkit)

### Find-AdmPwdExtendedRights

```pwsh
PS C:\Users\htb-student> Find-AdmPwdExtendedRights
```

### Find-LAPSDelegatedGroups

```pwsh
PS C:\Users\htb-student> Find-LAPSDelegatedGroups
```

### Get-LAPSComputers

```pwsh
PS C:\Users\htb-student> Get-LAPSComputers
```
