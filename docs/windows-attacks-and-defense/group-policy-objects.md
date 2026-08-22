# Group Policy Objects

## Event Viewer ([5136](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-5136))

```xml title="Details" hl_lines="4 22 27 28"
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" />
  <EventID>5136</EventID>
  <Version>0</Version>
  <Level>0</Level>
  <Task>14081</Task>
  <Opcode>0</Opcode>
  <Keywords>0x8020000000000000</Keywords>
  <TimeCreated SystemTime="2025-10-20T20:34:18.072995100Z" />
  <EventRecordID>556371</EventRecordID>
  <Correlation />
  <Execution ProcessID="680" ThreadID="796" />
  <Channel>Security</Channel>
  <Computer>DC1.eagle.local</Computer>
  <Security />
  </System>
- <EventData>
  <Data Name="OpCorrelationID">{28b9a209-a49b-4e8c-8bac-c2c4fc9c50ef}</Data>
  <Data Name="AppCorrelationID">-</Data>
  <Data Name="SubjectUserSid">S-1-5-21-1518138621-4282902758-752445584-5602</Data>
  <Data Name="SubjectUserName">htb-student</Data>
  <Data Name="SubjectDomainName">EAGLE</Data>
  <Data Name="SubjectLogonId">0xcc21f</Data>
  <Data Name="DSName">eagle.local</Data>
  <Data Name="DSType">%%14676</Data>
  <Data Name="ObjectDN">cn={73C66DBB-81DA-44D8-BDEF-20BA2C27056D},cn=policies,cn=system,DC=eagle,DC=local</Data>
  <Data Name="ObjectGUID">{2bada46d-26d1-4608-a8cb-b776c19be694}</Data>
  <Data Name="ObjectClass">groupPolicyContainer</Data>
  <Data Name="AttributeLDAPDisplayName">nTSecurityDescriptor</Data>
  <Data Name="AttributeSyntaxOID">2.5.5.15</Data>
  <Data Name="AttributeValue">O:DAG:DAD:PAI(OA;CI;CR;edacfd8f-ffb3-11d1-b41d-00a0c968f939;;AU)(A;;CCDCLCSWRPWPDTLOSDRCWDWO;;;DA)(A;CI;LCRPRC;;;S-1-5-21-1518138621-4282902758-752445584-1107)(A;CI;CCDCLCRPWPRC;;;DU)(A;CI;CCDCLCSWRPWPDTLOSDRCWDWO;;;DA)(A;CI;CCDCLCSWRPWPDTLOSDRCWDWO;;;S-1-5-21-1518138621-4282902758-752445584-519)(A;CI;LCRPLORC;;;ED)(A;CI;LCRPLORC;;;AU)(A;CI;CCDCLCSWRPWPDTLOSDRCWDWO;;;SY)(A;CIIO;CCDCLCSWRPWPDTLOSDRCWDWO;;;CO)S:AI(OU;CIIDSA;WPWD;;f30e3bc2-9ff0-11d1-b603-0000f80367c1;WD)(OU;CIIOIDSA;WP;f30e3bbe-9ff0-11d1-b603-0000f80367c1;bf967aa5-0de6-11d0-a285-00aa003049e2;WD)(OU;CIIOIDSA;WP;f30e3bbf-9ff0-11d1-b603-0000f80367c1;bf967aa5-0de6-11d0-a285-00aa003049e2;WD)</Data>
  <Data Name="OperationType">%%14674</Data>
  </EventData>
  </Event>
```

## Honeypot

* GPO kritik olmayan sunuculara bağlanmalıdır.
* GPO üzerinde yapılan değişiklikler izlenmelidir.
