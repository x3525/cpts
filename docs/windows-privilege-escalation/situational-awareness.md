# Situational Awareness

## Network Configuration

```pwsh
PS C:\Users\htb-student> ipconfig /all
```

```output title="Output" hl_lines="10 26"
Windows IP Configuration

   Host Name . . . . . . . . . . . . : WINLPE-SRV01
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : htb

Ethernet adapter Ethernet1:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : vmxnet3 Ethernet Adapter
   Physical Address. . . . . . . . . : 00-50-56-B0-51-0B
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::5196:ca37:70a7:5de9%2(Preferred)
   IPv4 Address. . . . . . . . . . . : 172.16.20.45(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.254.0
   Default Gateway . . . . . . . . . : 172.16.20.1
   DHCPv6 IAID . . . . . . . . . . . : 151015510
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2F-E3-AA-44-00-50-56-B0-51-0B
   DNS Servers . . . . . . . . . . . : 8.8.8.8
   NetBIOS over Tcpip. . . . . . . . : Enabled

Ethernet adapter Ethernet0 2:

   Connection-specific DNS Suffix  . : .htb
   Description . . . . . . . . . . . : vmxnet3 Ethernet Adapter #2
   Physical Address. . . . . . . . . : 00-50-56-B0-A7-01
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv6 Address. . . . . . . . . . . : dead:beef::111(Preferred)
   Lease Obtained. . . . . . . . . . : Tuesday, June 17, 2025 3:53:33 PM
   Lease Expires . . . . . . . . . . : Tuesday, June 17, 2025 4:53:32 PM
   IPv6 Address. . . . . . . . . . . : dead:beef::add8:f5c1:76a0:4106(Preferred)
   Link-local IPv6 Address . . . . . : fe80::add8:f5c1:76a0:4106%4(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.129.58.16(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Lease Obtained. . . . . . . . . . : Tuesday, June 17, 2025 3:53:46 PM
   Lease Expires . . . . . . . . . . : Tuesday, June 17, 2025 4:53:45 PM
   Default Gateway . . . . . . . . . : fe80::250:56ff:feb9:e928%4
                                       10.129.0.1
   DHCP Server . . . . . . . . . . . : 10.129.0.1
   DHCPv6 IAID . . . . . . . . . . . : 436228182
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2F-E3-AA-44-00-50-56-B0-51-0B
   DNS Servers . . . . . . . . . . . : 1.1.1.1
                                       8.8.8.8
   NetBIOS over Tcpip. . . . . . . . : Enabled
   Connection-specific DNS Suffix Search List :
                                       htb

Ethernet adapter Ethernet:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Windscribe VPN
   Physical Address. . . . . . . . . : 00-FF-B3-3C-CD-5A
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes

Tunnel adapter isatap..htb:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . : .htb
   Description . . . . . . . . . . . : Microsoft ISATAP Adapter
   Physical Address. . . . . . . . . : 00-00-00-00-00-00-00-E0
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes

Tunnel adapter isatap.{02D6F04C-A625-49D1-A85D-4FB454FBB3DB}:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft ISATAP Adapter #3
   Physical Address. . . . . . . . . : 00-00-00-00-00-00-00-E0
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
```

## ARP Table

```pwsh
PS C:\Users\htb-student> arp -a
```

```output title="Output"
Interface: 172.16.20.45 --- 0x2
  Internet Address      Physical Address      Type
  172.16.21.255         ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static

Interface: 10.129.58.16 --- 0x4
  Internet Address      Physical Address      Type
  10.129.0.1            00-50-56-b9-e9-28     dynamic
  10.129.255.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static
```

## Routing Table

```pwsh
PS C:\Users\htb-student> route print
```

```output title="Output"
===========================================================================
Interface List
  2...00 50 56 b0 51 0b ......vmxnet3 Ethernet Adapter
  4...00 50 56 b0 a7 01 ......vmxnet3 Ethernet Adapter #2
  7...00 ff b3 3c cd 5a ......Windscribe VPN
  1...........................Software Loopback Interface 1
  9...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter
  6...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
===========================================================================

IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0      172.16.20.1     172.16.20.45    271
          0.0.0.0          0.0.0.0       10.129.0.1     10.129.58.16     15
       10.129.0.0      255.255.0.0         On-link      10.129.58.16    271
     10.129.58.16  255.255.255.255         On-link      10.129.58.16    271
   10.129.255.255  255.255.255.255         On-link      10.129.58.16    271
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      172.16.20.0    255.255.254.0         On-link      172.16.20.45    271
     172.16.20.45  255.255.255.255         On-link      172.16.20.45    271
    172.16.21.255  255.255.255.255         On-link      172.16.20.45    271
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link      10.129.58.16    271
        224.0.0.0        240.0.0.0         On-link      172.16.20.45    271
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link      10.129.58.16    271
  255.255.255.255  255.255.255.255         On-link      172.16.20.45    271
===========================================================================
Persistent Routes:
  Network Address          Netmask  Gateway Address  Metric
          0.0.0.0          0.0.0.0      172.16.20.1  Default
===========================================================================

IPv6 Route Table
===========================================================================
Active Routes:
 If Metric Network Destination      Gateway
  4    271 ::/0                     fe80::250:56ff:feb9:e928
  1    331 ::1/128                  On-link
  4    271 dead:beef::/64           On-link
  4    271 dead:beef::111/128       On-link
  4    271 dead:beef::add8:f5c1:76a0:4106/128
                                    On-link
  4    271 fe80::/64                On-link
  2    271 fe80::/64                On-link
  2    271 fe80::5196:ca37:70a7:5de9/128
                                    On-link
  4    271 fe80::add8:f5c1:76a0:4106/128
                                    On-link
  1    331 ff00::/8                 On-link
  4    271 ff00::/8                 On-link
  2    271 ff00::/8                 On-link
===========================================================================
Persistent Routes:
  None
```

## Anti-Malware Status

```pwsh
PS C:\Users\htb-student> Get-MpComputerStatus
```

```output title="Output" hl_lines="6-8 14 31"
AMEngineVersion                 : 1.1.18400.4
AMProductVersion                : 4.10.14393.0
AMServiceEnabled                : True
AMServiceVersion                : 4.10.14393.0
AntispywareEnabled              : True
AntispywareSignatureAge         : 1410
AntispywareSignatureLastUpdated : 8/7/2021 11:16:31 AM
AntispywareSignatureVersion     : 1.345.139.0
AntivirusEnabled                : True
AntivirusSignatureAge           : 1410
AntivirusSignatureLastUpdated   : 8/7/2021 11:16:31 AM
AntivirusSignatureVersion       : 1.345.139.0
BehaviorMonitorEnabled          : False
ComputerID                      : 54AF7DE4-3C7E-4DA0-87AC-831B045B9063
ComputerState                   : 0
FullScanAge                     : 4294967295
FullScanEndTime                 :
FullScanStartTime               :
IoavProtectionEnabled           : False
LastFullScanSource              : 0
LastQuickScanSource             : 2
NISEnabled                      : False
NISEngineVersion                : 0.0.0.0
NISSignatureAge                 : 4294967295
NISSignatureLastUpdated         :
NISSignatureVersion             : 0.0.0.0
OnAccessProtectionEnabled       : False
QuickScanAge                    : 1409
QuickScanEndTime                : 8/7/2021 6:53:37 PM
QuickScanStartTime              : 8/7/2021 6:51:29 PM
RealTimeProtectionEnabled       : False
RealTimeScanDirection           : 0
PSComputerName                  :
```
