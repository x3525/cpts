# Subdomain Fuzzing

## Ffuf

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u "FUZZ.inlanefreight.com"
```

```output title="Output"
www                     [Status: 301, Size: 325, Words: 20, Lines: 10, Duration: 56ms]
support                 [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 272ms]
blog                    [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 330ms]
ns3                     [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 331ms]
my                      [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 134ms]
customer                [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 137ms]
```

## Gobuster

```sh
my@attack:~$ gobuster dns -w /usr/local/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -d "inlanefreight.com"
```

```output title="Output"
===============================================================
Starting gobuster in DNS enumeration mode
===============================================================
Found: www.inlanefreight.com

Found: ns1.inlanefreight.com

Found: blog.inlanefreight.com

Found: ns3.inlanefreight.com

Found: ns2.inlanefreight.com

Found: support.inlanefreight.com

Found: my.inlanefreight.com

Found: customer.inlanefreight.com
```
