# Directory Fuzzing

## Ffuf

| OPTION | DESCRIPTION |
|---|---|
| -ic | Wordlist yorumlarını göz ardı et. |
| -w | Wordlist konumu. Birden fazla olabilir. Takma isim verilebilir. |
| -u | Hedef URL. |

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt:FUZZ -u "http://94.237.59.180:32644/FUZZ"
```

```output title="Output"
blog                    [Status: 301, Size: 324, Words: 20, Lines: 10, Duration: 64ms]
forum                   [Status: 301, Size: 325, Words: 20, Lines: 10, Duration: 64ms]
```

## Gobuster

```sh
my@attack:~$ gobuster dir -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt -u "http://94.237.59.180:32644"
```

```output title="Output"
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/blog                 (Status: 301) [Size: 322] [--> http://94.237.59.180:32644/blog/]
/forum                (Status: 301) [Size: 323] [--> http://94.237.59.180:32644/forum/]
```

## Wenum

```sh
my@attack:~$ wenum -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt -u "http://94.237.59.180:32644/FUZZ"
```

```output title="Output"
 Code    Lines     Words        Size  Method   URL
──────────────────────────────────────────────────────────────────────────── Response number 1: ────────────────────────────────────────────────────────────────────────────
 200      55 L      85 W       986 B  GET      http://94.237.59.180:32644/
─────────────────────────────────────────────────────────────────────────── Response number 68: ────────────────────────────────────────────────────────────────────────────
 301       9 L      28 W       323 B  GET      http://94.237.59.180:32644/forum ➡ http://94.237.59.180:32644/forum/
─────────────────────────────────────────────────────────────────────────── Response number 33: ────────────────────────────────────────────────────────────────────────────
 301       9 L      28 W       322 B  GET      http://94.237.59.180:32644/blog ➡ http://94.237.59.180:32644/blog/
```
