# RSAT

## Available Tools

```pwsh
PS C:\Users\htb-student> Get-WindowsCapability -Name RSAT* -Online | Select-Object name,state
```

## Installing All Available Tools

```pwsh
PS C:\Users\htb-student> Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability –Online
```

## Installing a Specific Tool

```pwsh
PS C:\Users\htb-student> Add-WindowsCapability -Name Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0 –Online
```
