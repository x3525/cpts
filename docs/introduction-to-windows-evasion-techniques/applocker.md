# AppLocker

## Enumerating the Folders

```powershell title="AppLockerBypassChecker.ps1" linenums="1" hl_lines="8"
Get-ChildItem $env:windir -Recurse -ErrorAction SilentlyContinue | ForEach-Object {
    $dir = $_;
    (Get-Acl $dir.FullName).Access | ForEach-Object {
        if ($_.AccessControlType -eq "Allow")
        {
            if ($_.IdentityReference.Value -eq "NT AUTHORITY\Authenticated Users" -or $_.IdentityReference.Value -eq "BUILTIN\Users")
            {
                if (($_.FileSystemRights -like "*Write*" -or $_.FileSystemRights -like "*Create*") -and $_.FileSystemRights -like "*Execute*")
                {
                    Write-Host ($dir.FullName + ": " + $_.IdentityReference.Value + " (" + $_.FileSystemRights + ")");
                }
            }
        }
    };
}
```

```pwsh
PS C:\Users\alpha> C:\Tools\AppLockerBypassChecker.ps1
```

## Enumerating the Policy

```pwsh
PS C:\Users\alpha> Get-AppLockerPolicy -Effective | Select-Object -ExpandProperty RuleCollections
```

## Testing the Policy

### Allowed

```pwsh
PS C:\Users\alpha> Get-AppLockerPolicy -Effective | Test-AppLockerPolicy -Path C:\Tools\SysinternalsSuite\procexp.exe -User gamma
```

```output title="Output"
FilePath                               PolicyDecision MatchingRule
--------                               -------------- ------------
C:\Tools\SysinternalsSuite\procexp.exe        Allowed (Default Rule) All files
```

### DeniedByDefault

```pwsh
PS C:\Users\alpha> Get-AppLockerPolicy -Effective | Test-AppLockerPolicy -Path C:\Tools\SysinternalsSuite\procexp.exe -User delta
```

```output title="Output"
FilePath                                PolicyDecision MatchingRule
--------                                -------------- ------------
C:\Tools\SysinternalsSuite\procexp.exe DeniedByDefault
```
