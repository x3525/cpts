# PowerShell Defender Module

## Anti-Malware Status

```pwsh
PS C:\Users\Administrator> Get-MpComputerStatus
```

```output title="Output" hl_lines="7-9 15 29-30 45"
AMEngineVersion                  : 1.1.24030.4
AMProductVersion                 : 4.18.24030.9
AMRunningMode                    : Normal
AMServiceEnabled                 : True
AMServiceVersion                 : 4.18.24030.9
AntispywareEnabled               : True
AntispywareSignatureAge          : 278
AntispywareSignatureLastUpdated  : 5/6/2024 1:09:46 AM
AntispywareSignatureVersion      : 1.409.717.0
AntivirusEnabled                 : True
AntivirusSignatureAge            : 278
AntivirusSignatureLastUpdated    : 5/6/2024 1:09:46 AM
AntivirusSignatureVersion        : 1.409.717.0
BehaviorMonitorEnabled           : True
ComputerID                       : B6F71055-05CB-4BCE-A823-507103C278EA
ComputerState                    : 0
DefenderSignaturesOutOfDate      : False
DeviceControlDefaultEnforcement  :
DeviceControlPoliciesLastUpdated : 12/31/1600 4:00:00 PM
DeviceControlState               : Disabled
FullScanAge                      : 4294967295
FullScanEndTime                  :
FullScanOverdue                  : False
FullScanRequired                 : False
FullScanSignatureVersion         :
FullScanStartTime                :
InitializationProgress           : ServiceStartedSuccessfully
IoavProtectionEnabled            : True
IsTamperProtected                : True
IsVirtualMachine                 : True
LastFullScanSource               : 0
LastQuickScanSource              : 2
NISEnabled                       : True
NISEngineVersion                 : 1.1.24030.4
NISSignatureAge                  : 278
NISSignatureLastUpdated          : 5/6/2024 1:09:46 AM
NISSignatureVersion              : 1.409.717.0
OnAccessProtectionEnabled        : True
ProductStatus                    : 524288
QuickScanAge                     : 278
QuickScanEndTime                 : 5/6/2024 6:03:03 AM
QuickScanOverdue                 : False
QuickScanSignatureVersion        : 1.409.700.0
QuickScanStartTime               : 5/6/2024 6:00:56 AM
RealTimeProtectionEnabled        : True
RealTimeScanDirection            : 0
RebootRequired                   : False
SmartAppControlExpiration        :
SmartAppControlState             : Off
TamperProtectionSource           : UI
TDTCapable                       : N/A
TDTMode                          : N/A
TDTSiloType                      : N/A
TDTStatus                        : N/A
TDTTelemetry                     : N/A
TroubleShootingDailyMaxQuota     :
TroubleShootingDailyQuotaLeft    :
TroubleShootingEndTime           :
TroubleShootingExpirationLeft    :
TroubleShootingMode              :
TroubleShootingModeSource        :
TroubleShootingQuotaResetTime    :
TroubleShootingStartTime         :
PSComputerName                   :
```

## [Get-MpThreat](https://learn.microsoft.com/en-us/powershell/module/defender/get-mpthreat?view=windowsserver2025-ps)

```pwsh
PS C:\Users\Administrator> Get-MpThreat
```

```output title="Output" title="Output" hl_lines="9"
CategoryID       : 8
DidThreatExecute : False
IsActive         : True
Resources        :
RollupStatus     : 1
SchemaVersion    : 1.0.0.0
SeverityID       : 5
ThreatID         : 2147775465
ThreatName       : Trojan:MSIL/TurtleLoader.A!dha
TypeID           : 0
PSComputerName   :
```

## [Get-MpThreatDetection](https://learn.microsoft.com/en-us/powershell/module/defender/get-mpthreatdetection?view=windowsserver2025-ps)

```pwsh
PS C:\Users\Administrator> Get-MpThreatDetection
```

```output title="Output" hl_lines="13"
ActionSuccess                  : True
AdditionalActionsBitMask       : 0
AMProductVersion               : 4.18.24030.9
CleaningActionID               : 9
CurrentThreatExecutionStatusID : 1
DetectionID                    : {6D167C59-D9A3-4AB8-9BF0-A188F47216F5}
DetectionSourceTypeID          : 3
DomainUser                     : EVASION-DEV\Administrator
InitialDetectionTime           : 2/8/2025 9:33:44 PM
LastThreatStatusChangeTime     : 2/8/2025 9:33:44 PM
ProcessName                    : C:\Windows\explorer.exe
RemediationTime                :
Resources                      : {file:_C:\Users\Administrator\Desktop\StaticAnalysis.exe}
ThreatID                       : 2147775465
ThreatStatusErrorCode          : 0
ThreatStatusID                 : 1
PSComputerName                 :
```

## Clearing Protection History

```pwsh
PS C:\Users\Administrator> Remove-Item -Recurse "C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service"
```

## Disabling Real-Time Protection

```pwsh
PS C:\Users\Administrator> Set-MpPreference -DisableRealTimeMonitoring $true
```
