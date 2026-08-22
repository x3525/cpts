# PowerView Overview

## Misc Functions

* [Export-PowerViewCSV](https://powersploit.readthedocs.io/en/latest/Recon/Export-PowerViewCSV/)
* [Resolve-IPAddress](https://powersploit.readthedocs.io/en/latest/Recon/Resolve-IPAddress/) (Get-IPAddress)
* [ConvertTo-SID](https://powersploit.readthedocs.io/en/latest/Recon/ConvertTo-SID/) (Convert-NameToSid)
* [ConvertFrom-SID](https://powersploit.readthedocs.io/en/latest/Recon/ConvertFrom-SID/) (Convert-SidToName)
* [Convert-ADName](https://powersploit.readthedocs.io/en/latest/Recon/Convert-ADName/)
* [ConvertFrom-UACValue](https://powersploit.readthedocs.io/en/latest/Recon/ConvertFrom-UACValue/)
* [Add-RemoteConnection](https://powersploit.readthedocs.io/en/latest/Recon/Add-RemoteConnection/)
* [Remove-RemoteConnection](https://powersploit.readthedocs.io/en/latest/Recon/Remove-RemoteConnection/)
* [Invoke-UserImpersonation](https://powersploit.readthedocs.io/en/latest/Recon/Invoke-UserImpersonation/)
* [Invoke-RevertToSelf](https://powersploit.readthedocs.io/en/latest/Recon/Invoke-RevertToSelf/)
* [Get-DomainSPNTicket](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainSPNTicket/) (Request-SPNTicket)
* [Invoke-Kerberoast](https://powersploit.readthedocs.io/en/latest/Recon/Invoke-Kerberoast/)
* [Get-PathAcl](https://powersploit.readthedocs.io/en/latest/Recon/Get-PathAcl/)

## Domain and LDAP Functions

* [Get-DomainDNSZone](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainDNSZone/) (Get-DNSZone)
* [Get-DomainDNSRecord](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainDNSRecord/) (Get-DNSRecord)
* [Get-Domain](https://powersploit.readthedocs.io/en/latest/Recon/Get-Domain/) (Get-NetDomain)
* [Get-DomainController](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainController/) (Get-NetDomainController)
* [Get-Forest](https://powersploit.readthedocs.io/en/latest/Recon/Get-Forest/) (Get-NetForest)
* [Get-ForestDomain](https://powersploit.readthedocs.io/en/latest/Recon/Get-ForestDomain/) (Get-NetForestDomain)
* [Get-ForestGlobalCatalog](https://powersploit.readthedocs.io/en/latest/Recon/Get-ForestGlobalCatalog/) (Get-NetForestCatalog)
* [Find-DomainObjectPropertyOutlier](https://powersploit.readthedocs.io/en/latest/Recon/Find-DomainObjectPropertyOutlier/)
* [Get-DomainUser](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainUser/) (Get-NetUser)
* [New-DomainUser](https://powersploit.readthedocs.io/en/latest/Recon/New-DomainUser/)
* [Set-DomainUserPassword](https://powersploit.readthedocs.io/en/latest/Recon/Set-DomainUserPassword/)
* [Get-DomainUserEvent](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainUserEvent/) (Get-UserEvent)
* [Get-DomainComputer](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainComputer/) (Get-NetComputer)
* [Get-DomainObject](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainObject/) (Get-ADObject)
* [Set-DomainObject](https://powersploit.readthedocs.io/en/latest/Recon/Set-DomainObject/) (Set-ADObject)
* [Set-DomainObjectOwner](https://powersploit.readthedocs.io/en/latest/Recon/Set-DomainObjectOwner/)
* [Get-DomainObjectAcl](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainObjectAcl/) (Get-ObjectAcl)
* [Add-DomainObjectAcl](https://powersploit.readthedocs.io/en/latest/Recon/Add-DomainObjectAcl/) (Add-ObjectAcl)
* [Find-InterestingDomainAcl](https://powersploit.readthedocs.io/en/latest/Recon/Find-InterestingDomainAcl/) (Invoke-ACLScanner)
* [Get-DomainOU](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainOU/) (Get-NetOU)
* [Get-DomainSite](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainSite/) (Get-NetSite)
* [Get-DomainSubnet](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainSubnet/) (Get-NetSubnet)
* [Get-DomainSID](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainSID/)
* [Get-DomainGroup](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroup/) (Get-NetGroup)
* [New-DomainGroup](https://powersploit.readthedocs.io/en/latest/Recon/New-DomainGroup/)
* [Get-DomainManagedSecurityGroup](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainManagedSecurityGroup/) (Find-ManagedSecurityGroups)
* [Get-DomainGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGroupMember/) (Get-NetGroupMember)
* [Add-DomainGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Add-DomainGroupMember/)
* [Get-DomainFileServer](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainFileServer/) (Get-NetFileServer)
* [Get-DomainDFSShare](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainDFSShare/) (Get-DFSshare)

## GPO Functions

* [Get-DomainGPO](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGPO/) (Get-NetGPO)
* [Get-DomainGPOLocalGroup](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGPOLocalGroup/) (Get-NetGPOGroup)
* [Get-DomainGPOUserLocalGroupMapping](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGPOUserLocalGroupMapping/) (Find-GPOLocation)
* [Get-DomainGPOComputerLocalGroupMapping](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainGPOComputerLocalGroupMapping/) (Find-GPOComputerAdmin)
* [Get-DomainPolicyData](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainPolicy/) (Get-DomainPolicy)

## Computer Enumeration Functions

* [Get-NetLocalGroup](https://powersploit.readthedocs.io/en/latest/Recon/Get-NetLocalGroup/)
* [Get-NetLocalGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Get-NetLocalGroupMember/)
* [Get-NetShare](https://powersploit.readthedocs.io/en/latest/Recon/Get-NetShare/)
* [Get-NetLoggedon](https://powersploit.readthedocs.io/en/latest/Recon/Get-NetLoggedon/)
* [Get-NetSession](https://powersploit.readthedocs.io/en/latest/Recon/Get-NetSession/)
* [Get-RegLoggedOn](https://powersploit.readthedocs.io/en/latest/Recon/Get-RegLoggedOn/) (Get-LoggedOnLocal)
* [Get-NetRDPSession](https://powersploit.readthedocs.io/en/latest/Recon/Get-NetRDPSession/)
* [Test-AdminAccess](https://powersploit.readthedocs.io/en/latest/Recon/Test-AdminAccess/) (Invoke-CheckLocalAdminAccess)
* [Get-NetComputerSiteName](https://powersploit.readthedocs.io/en/latest/Recon/Get-NetComputerSiteName/) (Get-SiteName)
* [Get-WMIRegProxy](https://powersploit.readthedocs.io/en/latest/Recon/Get-WMIRegProxy/) (Get-Proxy)
* [Get-WMIRegLastLoggedOn](https://powersploit.readthedocs.io/en/latest/Recon/Get-WMIRegLastLoggedOn/) (Get-LastLoggedOn)
* [Get-WMIRegCachedRDPConnection](https://powersploit.readthedocs.io/en/latest/Recon/Get-WMIRegCachedRDPConnection/) (Get-CachedRDPConnection)
* [Get-WMIRegMountedDrive](https://powersploit.readthedocs.io/en/latest/Recon/Get-WMIRegMountedDrive/) (Get-RegistryMountedDrive)
* [Get-WMIProcess](https://powersploit.readthedocs.io/en/latest/Recon/Get-WMIProcess/) (Get-NetProcess)
* [Find-InterestingFile](https://powersploit.readthedocs.io/en/latest/Recon/Find-InterestingFile/)

## Threaded Meta Functions

* [Find-DomainUserLocation](https://powersploit.readthedocs.io/en/latest/Recon/Find-DomainUserLocation/) (Invoke-UserHunter)
* [Find-DomainProcess](https://powersploit.readthedocs.io/en/latest/Recon/Find-DomainProcess/) (Invoke-ProcessHunter)
* [Find-DomainUserEvent](https://powersploit.readthedocs.io/en/latest/Recon/Find-DomainUserEvent/) (Invoke-EventHunter)
* [Find-DomainShare](https://powersploit.readthedocs.io/en/latest/Recon/Find-DomainShare/) (Invoke-ShareFinder)
* [Find-InterestingDomainShareFile](https://powersploit.readthedocs.io/en/latest/Recon/Find-InterestingDomainShareFile/) (Invoke-FileFinder)
* [Find-LocalAdminAccess](https://powersploit.readthedocs.io/en/latest/Recon/Find-LocalAdminAccess/)
* [Find-DomainLocalGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Find-DomainLocalGroupMember/) (Invoke-EnumerateLocalAdmin)

## Domain Trust Functions

* [Get-DomainTrust](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainTrust/) (Get-NetDomainTrust)
* [Get-ForestTrust](https://powersploit.readthedocs.io/en/latest/Recon/Get-ForestTrust/) (Get-NetForestTrust)
* [Get-DomainForeignUser](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainForeignUser/) (Find-ForeignUser)
* [Get-DomainForeignGroupMember](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainForeignGroupMember/) (Find-ForeignGroup)
* [Get-DomainTrustMapping](https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainTrustMapping/) (Invoke-MapDomainTrust)
