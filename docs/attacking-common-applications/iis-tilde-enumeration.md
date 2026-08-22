# IIS Tilde Enumeration

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.244.173 -p- --open -sV -sC
```

```output title="Output" hl_lines="7"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-12 11:19 +03
Nmap scan report for 10.129.244.147
Host is up (0.14s latency).
Not shown: 65534 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 7.5
| http-methods:
|_  Potentially risky methods: TRACE
|_http-title: Bounty
|_http-server-header: Microsoft-IIS/7.5
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 380.51 seconds
```

## [IIS-ShortName-Scanner](https://github.com/irsdl/IIS-ShortName-Scanner)

```sh
my@attack:~$ git clone https://github.com/irsdl/IIS-ShortName-Scanner.git
my@attack:~$ cd IIS-ShortName-Scanner/release
my@attack:~/IIS-ShortName-Scanner/release$ java -jar iis_shortname_scanner.jar 0 5 "http://10.129.244.147"
```

```output title="Output" hl_lines="12-13 15 17"
Early result: the target is probably vulnerable.
Early result: identified letters in names > A,C,D,E,F,L,N,O,P,R,S,T,U,X
Early result: identified letters in extensions > A,C,P,S
# IIS Short Name (8.3) Scanner version 2023.4 - scan initiated 2025/05/12 11:34:06
Target: http://10.129.244.147/
|_ Result: Vulnerable!
|_ Used HTTP method: OPTIONS
|_ Suffix (magic part): /~1/.rem
|_ Extra information:
  |_ Number of sent requests: 571
  |_ Identified directories: 2
    |_ ASPNET~1
    |_ UPLOAD~1
  |_ Identified files: 2
    |_ CSASPX~1.CS
      |_ Actual extension = .CS
    |_ TRANSF~1.ASP
```

## Creating a Wordlist

```sh
my@attack:~$ grep -rhE '^transf' /usr/local/share/wordlists | sort -u > wordlist.txt
```

## Fuzzing

```sh
my@attack:~$ ffuf -ic -w wordlist.txt:FUZZ -u "http://10.129.244.147/FUZZ" -e .asp,.aspx
```

```output title="Output"
transfer.aspx           [Status: 200, Size: 941, Words: 89, Lines: 22, Duration: 204ms]
```
