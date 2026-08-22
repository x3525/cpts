# Tomcat

## Version

```sh
my@attack:~$ curl -s 'http://web01.inlanefreight.local:8180/docs/' -L | html2text | head
```

```output title="Output" hl_lines="8"
[![Tomcat Home](./images/tomcat.png)](https://tomcat.apache.org/)

[![The Apache Software Foundation](./images/asf-
logo.svg)](https://www.apache.org/)

# Apache Tomcat 10

Version 10.0.10, Jul 30 2021

## Links
```

## Version on Error

```sh
my@attack:~$ curl -s 'http://app-dev.inlanefreight.local:8080/NonExistingDirectory/' | html2text | grep Tomcat

```

```output title="Output"
### Apache Tomcat/9.0.30
```

## Enumeration

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt:FUZZ -u "http://web01.inlanefreight.local:8180/FUZZ"
```

```output title="Output"
docs                    [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 3595ms]
examples                [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 200ms]
manager                 [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 133ms]
```
