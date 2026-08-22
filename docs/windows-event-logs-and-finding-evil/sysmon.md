# Sysmon

## Installing

```pwsh
PS C:\Users\Administrator\Desktop> .\Sysmon.exe -accepteula -i -h * -l -n
```

## Utilizing [Custom Configuration](https://github.com/SwiftOnSecurity/sysmon-config/blob/master/sysmonconfig-export.xml)

```pwsh
PS C:\Users\Administrator\Desktop> .\Sysmon.exe -c sysmonconfig-export.xml
```

## Detecting DLL Hijacking

### Editing Configuration to Include [Sysmon Event ID 7](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90007)

=== "BEFORE"

    ```xml title="sysmonconfig-export.xml" linenums="425"
    <ImageLoad onmatch="include">
        <!--NOTE: Using "include" with no rules means nothing in this section will be logged-->
    </ImageLoad>
    ```

=== "AFTER"

    ```xml title="sysmonconfig-export.xml" linenums="425"
    <ImageLoad onmatch="exclude">
        <!--NOTE: Using "include" with no rules means nothing in this section will be logged-->
    </ImageLoad>
    ```

```pwsh
PS C:\Users\Administrator> .\Sysmon.exe -c sysmonconfig-export.xml
```

### Creating the Event Log ([Hijacking](https://hijacklibs.net/#calc.exe))

* [wininet.dll](https://hijacklibs.net/entries/microsoft/built-in/wininet.html)
* windows.storage.dll
* twinui.appcore.dll
* sspicli.dll
* secur32.dll
* propsys.dll
* mlang.dll
* execmodelproxy.dll
* edputil.dll
* cryptbase.dll

```pwsh
PS C:\Users\Administrator\Desktop> git clone https://github.com/stephenfewer/ReflectiveDLLInjection.git
PS C:\Users\Administrator\Desktop> copy ReflectiveDLLInjection\bin\reflective_dll.x64.dll wininet.dll
PS C:\Users\Administrator\Desktop> copy C:\Windows\System32\calc.exe calc.exe
PS C:\Users\Administrator\Desktop> .\calc.exe
```

### Event Viewer (Event ID 7)

1. Windows Logs
2. Application and Services Logs
3. Microsoft
4. Windows
5. Sysmon
6. Operational
7. ++right-button++
8. Filter Current Log
    * Filter
    * Includes EventIDs: 7
9. Action
10. Find
    * Find what: calc.exe
    * Find Next

```xml title="Details" hl_lines="23 24 33"
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Sysmon" Guid="{5770385f-c22a-43e0-bf4c-06f5698ffbd9}" />
  <EventID>7</EventID>
  <Version>3</Version>
  <Level>4</Level>
  <Task>7</Task>
  <Opcode>0</Opcode>
  <Keywords>0x8000000000000000</Keywords>
  <TimeCreated SystemTime="2025-10-16T13:27:52.2824733Z" />
  <EventRecordID>32827</EventRecordID>
  <Correlation />
  <Execution ProcessID="2888" ThreadID="3716" />
  <Channel>Microsoft-Windows-Sysmon/Operational</Channel>
  <Computer>DESKTOP-NU10MTO</Computer>
  <Security UserID="S-1-5-18" />
  </System>
- <EventData>
  <Data Name="RuleName">-</Data>
  <Data Name="UtcTime">2025-10-16 13:27:52.217</Data>
  <Data Name="ProcessGuid">{52ff3419-f2d6-68f0-6a01-000000001000}</Data>
  <Data Name="ProcessId">6508</Data>
  <Data Name="Image">C:\Users\Administrator\Desktop\calc.exe</Data>
  <Data Name="ImageLoaded">C:\Users\Administrator\Desktop\wininet.dll</Data>
  <Data Name="FileVersion">-</Data>
  <Data Name="Description">-</Data>
  <Data Name="Product">-</Data>
  <Data Name="Company">-</Data>
  <Data Name="OriginalFileName">-</Data>
  <Data Name="Hashes">MD5=D4990A8D2FF6F2433ACDAD04521F85C6,SHA256=51F2305DCF385056C68F7CCF5B1B3B9304865CEF1257947D4AD6EF5FAD2E3B13,IMPHASH=FB1B9FF3C0BBBF95713D517725CEE833</Data>
  <Data Name="Signed">false</Data>
  <Data Name="Signature">-</Data>
  <Data Name="SignatureStatus">Unavailable</Data>
  <Data Name="User">DESKTOP-NU10MTO\Administrator</Data>
  </EventData>
  </Event>
```

## Detecting Credential Dumping

### Editing Configuration to Include [Sysmon Event ID 10](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90010)

=== "BEFORE"

    ```xml title="sysmonconfig-export.xml" linenums="472"
    <ProcessAccess onmatch="include">
        <!--NOTE: Using "include" with no rules means nothing in this section will be logged-->
    </ProcessAccess>
    ```

=== "AFTER"

    ```xml title="sysmonconfig-export.xml" linenums="472"
    <ProcessAccess onmatch="exclude">
        <!--NOTE: Using "include" with no rules means nothing in this section will be logged-->
    </ProcessAccess>
    ```

```pwsh
PS C:\Users\Administrator> .\Sysmon.exe -c sysmonconfig-export.xml
```

### Creating the Event Log (Dumping)

```pwsh
PS C:\Users\Administrator\Desktop> .\notMimi.exe privilege::debug "sekurlsa::logonpasswords" exit
```

### Event Viewer (Event ID 10)

1. Windows Logs
2. Application and Services Logs
3. Microsoft
4. Windows
5. Sysmon
6. Operational
7. ++right-button++
8. Filter Current Log
    * Filter
    * Includes EventIDs: 10
9. Action
10. Find
    * Find what: notMimi.exe
    * Find Next

```xml title="Details" hl_lines="24 27 30 31"
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Sysmon" Guid="{5770385f-c22a-43e0-bf4c-06f5698ffbd9}" />
  <EventID>10</EventID>
  <Version>3</Version>
  <Level>4</Level>
  <Task>10</Task>
  <Opcode>0</Opcode>
  <Keywords>0x8000000000000000</Keywords>
  <TimeCreated SystemTime="2025-10-16T14:06:30.2357286Z" />
  <EventRecordID>43550</EventRecordID>
  <Correlation />
  <Execution ProcessID="2888" ThreadID="3744" />
  <Channel>Microsoft-Windows-Sysmon/Operational</Channel>
  <Computer>DESKTOP-NU10MTO</Computer>
  <Security UserID="S-1-5-18" />
  </System>
- <EventData>
  <Data Name="RuleName">-</Data>
  <Data Name="UtcTime">2025-10-16 14:06:30.232</Data>
  <Data Name="SourceProcessGUID">{52ff3419-fbe6-68f0-7b02-000000001000}</Data>
  <Data Name="SourceProcessId">3400</Data>
  <Data Name="SourceThreadId">1092</Data>
  <Data Name="SourceImage">C:\Users\Administrator\Desktop\notMimi.exe</Data>
  <Data Name="TargetProcessGUID">{52ff3419-ece1-68f0-0c00-000000001000}</Data>
  <Data Name="TargetProcessId">696</Data>
  <Data Name="TargetImage">C:\Windows\system32\lsass.exe</Data>
  <Data Name="GrantedAccess">0x1010</Data>
  <Data Name="CallTrace">C:\Windows\SYSTEM32\ntdll.dll+9d404|C:\Windows\System32\KERNELBASE.dll+2c13e|C:\Users\Administrator\Desktop\notMimi.exe+b6222|C:\Users\Administrator\Desktop\notMimi.exe+b65e5|C:\Users\Administrator\Desktop\notMimi.exe+b6161|C:\Users\Administrator\Desktop\notMimi.exe+838f4|C:\Users\Administrator\Desktop\notMimi.exe+8372c|C:\Users\Administrator\Desktop\notMimi.exe+83477|C:\Users\Administrator\Desktop\notMimi.exe+bcbc9|C:\Windows\System32\KERNEL32.DLL+17034|C:\Windows\SYSTEM32\ntdll.dll+52651</Data>
  <Data Name="SourceUser">DESKTOP-NU10MTO\Administrator</Data>
  <Data Name="TargetUser">NT AUTHORITY\SYSTEM</Data>
  </EventData>
  </Event>
```
