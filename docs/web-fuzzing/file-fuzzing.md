# File Fuzzing

## Finding File Extensions

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u "http://94.237.59.180:32644/blog/indexFUZZ"
```

```output title="Output"
.php                    [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 166ms]
.phps                   [Status: 403, Size: 282, Words: 20, Lines: 10, Duration: 2157ms]
```

## Finding Files

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt:FUZZ -u "http://94.237.59.180:32644/blog/FUZZ" -e .php
```

```output title="Output"
home.php                [Status: 200, Size: 1046, Words: 438, Lines: 58, Duration: 73ms]
index.php               [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 4799ms]
```
