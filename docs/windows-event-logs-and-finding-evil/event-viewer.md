# Event Viewer

## Filtering

1. Windows Logs
2. Security
3. ++right-button++
4. Filter Current Log
    * XML
    * Edit query manually

```xml title="Event Filter"
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">*[System[(EventID=4624) and TimeCreated[@SystemTime&gt;='2022-08-03T17:23:25.000Z']]]</Select>
  </Query>
</QueryList>
```

## [4624](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624)

```xml title="Details" hl_lines="4 10 22"
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" />
  <EventID>4624</EventID>
  <Version>2</Version>
  <Level>0</Level>
  <Task>12544</Task>
  <Opcode>0</Opcode>
  <Keywords>0x8020000000000000</Keywords>
  <TimeCreated SystemTime="2022-08-03T17:23:25.5466420Z" />
  <EventRecordID>5281</EventRecordID>
  <Correlation ActivityID="{37c34816-a75d-0001-7b48-c3375da7d801}" />
  <Execution ProcessID="668" ThreadID="720" />
  <Channel>Security</Channel>
  <Computer>DESKTOP-NU10MTO</Computer>
  <Security />
  </System>
- <EventData>
  <Data Name="SubjectUserSid">S-1-5-18</Data>
  <Data Name="SubjectUserName">DESKTOP-NU10MTO$</Data>
  <Data Name="SubjectDomainName">WORKGROUP</Data>
  <Data Name="SubjectLogonId">0x3e7</Data>
  <Data Name="TargetUserSid">S-1-5-18</Data>
  <Data Name="TargetUserName">SYSTEM</Data>
  <Data Name="TargetDomainName">NT AUTHORITY</Data>
  <Data Name="TargetLogonId">0x3e7</Data>
  <Data Name="LogonType">5</Data>
  <Data Name="LogonProcessName">Advapi</Data>
  <Data Name="AuthenticationPackageName">Negotiate</Data>
  <Data Name="WorkstationName">-</Data>
  <Data Name="LogonGuid">{00000000-0000-0000-0000-000000000000}</Data>
  <Data Name="TransmittedServices">-</Data>
  <Data Name="LmPackageName">-</Data>
  <Data Name="KeyLength">0</Data>
  <Data Name="ProcessId">0x288</Data>
  <Data Name="ProcessName">C:\Windows\System32\services.exe</Data>
  <Data Name="IpAddress">-</Data>
  <Data Name="IpPort">-</Data>
  <Data Name="ImpersonationLevel">%%1833</Data>
  <Data Name="RestrictedAdminMode">-</Data>
  <Data Name="TargetOutboundUserName">-</Data>
  <Data Name="TargetOutboundDomainName">-</Data>
  <Data Name="VirtualAccount">%%1843</Data>
  <Data Name="TargetLinkedLogonId">0x0</Data>
  <Data Name="ElevatedToken">%%1842</Data>
  </EventData>
  </Event>
```

## Filtering (SubjectLogonId)

1. Windows Logs
2. Security
3. ++right-button++
4. Filter Current Log
    * XML
    * Edit query manually

```xml title="Event Filter"
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='SubjectLogonId']='0x3E7']] and *[System[TimeCreated[@SystemTime&gt;='2022-08-03T17:23:25.000Z']]]</Select>
  </Query>
</QueryList>
```

## [4672](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4672)

```xml title="Details" hl_lines="4 23"
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" />
  <EventID>4672</EventID>
  <Version>0</Version>
  <Level>0</Level>
  <Task>12548</Task>
  <Opcode>0</Opcode>
  <Keywords>0x8020000000000000</Keywords>
  <TimeCreated SystemTime="2022-08-03T17:23:25.5466536Z" />
  <EventRecordID>5282</EventRecordID>
  <Correlation ActivityID="{37c34816-a75d-0001-7b48-c3375da7d801}" />
  <Execution ProcessID="668" ThreadID="720" />
  <Channel>Security</Channel>
  <Computer>DESKTOP-NU10MTO</Computer>
  <Security />
  </System>
- <EventData>
  <Data Name="SubjectUserSid">S-1-5-18</Data>
  <Data Name="SubjectUserName">SYSTEM</Data>
  <Data Name="SubjectDomainName">NT AUTHORITY</Data>
  <Data Name="SubjectLogonId">0x3e7</Data>
  <Data Name="PrivilegeList">SeAssignPrimaryTokenPrivilege SeTcbPrivilege SeSecurityPrivilege SeTakeOwnershipPrivilege SeLoadDriverPrivilege SeBackupPrivilege SeRestorePrivilege SeDebugPrivilege SeAuditPrivilege SeSystemEnvironmentPrivilege SeImpersonatePrivilege SeDelegateSessionUserImpersonatePrivilege</Data>
  </EventData>
  </Event>
```

## [4907](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4907)

```xml title="Details" hl_lines="4 30"
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" />
  <EventID>4907</EventID>
  <Version>0</Version>
  <Level>0</Level>
  <Task>13568</Task>
  <Opcode>0</Opcode>
  <Keywords>0x8020000000000000</Keywords>
  <TimeCreated SystemTime="2022-08-03T17:23:49.9561034Z" />
  <EventRecordID>5283</EventRecordID>
  <Correlation />
  <Execution ProcessID="4" ThreadID="5468" />
  <Channel>Security</Channel>
  <Computer>DESKTOP-NU10MTO</Computer>
  <Security />
  </System>
- <EventData>
  <Data Name="SubjectUserSid">S-1-5-18</Data>
  <Data Name="SubjectUserName">DESKTOP-NU10MTO$</Data>
  <Data Name="SubjectDomainName">WORKGROUP</Data>
  <Data Name="SubjectLogonId">0x3e7</Data>
  <Data Name="ObjectServer">Security</Data>
  <Data Name="ObjectType">File</Data>
  <Data Name="ObjectName">C:\Windows\WinSxS\FileMaps\_0000000000000000.cdf-ms</Data>
  <Data Name="HandleId">0xbb8</Data>
  <Data Name="OldSd" />
  <Data Name="NewSd">S:ARAI(AU;SAFA;0x1f0116;;;WD)</Data>
  <Data Name="ProcessId">0x8</Data>
  <Data Name="ProcessName">C:\Windows\WinSxS\amd64_microsoft-windows-servicingstack_31bf3856ad364e35_10.0.19041.1790_none_7df2aec07ca10e81\TiWorker.exe</Data>
  </EventData>
  </Event>
```
