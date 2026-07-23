# Recursive Fuzzing

## Ffuf

| OPTION | DESCRIPTION |
|---|---|
| -recursion | Özyinelemeli tarama. |
| -recursion-depth | Maksimum özyineleme derinliği. |
| -timeout | HTTP istek zaman aşımı. |
| -rate | Saniye başına gönderilen istek sayısı. |
| -v | Ayrıntılı çıktı. URL yönlendirme bilgileri gösterilir. |

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt:FUZZ -u "http://94.237.59.180:32644/FUZZ" -e .php -recursion -recursion-depth 1 -timeout 10 -rate 500 -v
```

```output title="Output" hl_lines="18 25"
[Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 75ms]
| URL | http://94.237.59.180:32644/
    * FUZZ:

[Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 75ms]
| URL | http://94.237.59.180:32644/index.php
    * FUZZ: index.php

[Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 77ms]
| URL | http://94.237.59.180:32644/.php
    * FUZZ: .php

[Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 64ms]
| URL | http://94.237.59.180:32644/forum
| --> | http://94.237.59.180:32644/forum/
    * FUZZ: forum

[INFO] Adding a new job to the queue: http://94.237.59.180:32644/forum/FUZZ

[Status: 301, Size: 322, Words: 20, Lines: 10, Duration: 4020ms]
| URL | http://94.237.59.180:32644/blog
| --> | http://94.237.59.180:32644/blog/
    * FUZZ: blog

[INFO] Adding a new job to the queue: http://94.237.59.180:32644/blog/FUZZ

[Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 64ms]
| URL | http://94.237.59.180:32644/.php
    * FUZZ: .php

[Status: 200, Size: 986, Words: 423, Lines: 56, Duration: 73ms]
| URL | http://94.237.59.180:32644/
    * FUZZ:

[INFO] Starting queued job on target: http://94.237.59.180:32644/forum/FUZZ

[Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 72ms]
| URL | http://94.237.59.180:32644/forum/.php
    * FUZZ: .php

[Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 74ms]
| URL | http://94.237.59.180:32644/forum/
    * FUZZ:

[Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 72ms]
| URL | http://94.237.59.180:32644/forum/index.php
    * FUZZ: index.php

[Status: 200, Size: 21, Words: 1, Lines: 1, Duration: 73ms]
| URL | http://94.237.59.180:32644/forum/flag.php
    * FUZZ: flag.php

[Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 65ms]
| URL | http://94.237.59.180:32644/forum/.php
    * FUZZ: .php

[Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 74ms]
| URL | http://94.237.59.180:32644/forum/
    * FUZZ:

[INFO] Starting queued job on target: http://94.237.59.180:32644/blog/FUZZ

[Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 66ms]
| URL | http://94.237.59.180:32644/blog/.php
    * FUZZ: .php

[Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 74ms]
| URL | http://94.237.59.180:32644/blog/
    * FUZZ:

[Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 75ms]
| URL | http://94.237.59.180:32644/blog/index.php
    * FUZZ: index.php

[Status: 200, Size: 1046, Words: 438, Lines: 58, Duration: 73ms]
| URL | http://94.237.59.180:32644/blog/home.php
    * FUZZ: home.php

[Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 65ms]
| URL | http://94.237.59.180:32644/blog/
    * FUZZ:

[Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 72ms]
| URL | http://94.237.59.180:32644/blog/.php
    * FUZZ: .php
```

## Wenum

| OPTION | DESCRIPTION |
|---|---|
| --recursion | Özyinelemeli tarama. |
| --request-timeout | HTTP istek zaman aşımı. |
| --location | Yönlendirmeleri takip etmek için kullanılır. |

```sh
my@attack:~$ wenum -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-small.txt -u "http://94.237.59.180:32644/FUZZ" -e .php --recursion 1 --request-timeout 10 --location
```

```output title="Output" hl_lines="13 19"
 Code    Lines     Words        Size  Method   URL
──────────────────────────────────────────────────────────────────────────── Response number 1: ────────────────────────────────────────────────────────────────────────────
 200      55 L      85 W       986 B  GET      http://94.237.59.180:32644/
─────────────────────────────────────────────────────────────────────────── Response number 64: ────────────────────────────────────────────────────────────────────────────
 301       9 L      28 W       320 B  GET      http://94.237.59.180:32644/blog ➡ http://94.237.59.180:32644/blog/
 RedirectQueue:        Added a request to follow redirection
─────────────────────────────────────────────────────────────────────────── Response number 29: ────────────────────────────────────────────────────────────────────────────
 403       9 L      28 W       280 B  GET      http://94.237.59.180:32644/.php
─────────────────────────────────────────────────────────────────────────── Response number 31: ────────────────────────────────────────────────────────────────────────────
 200      55 L      85 W       986 B  GET      http://94.237.59.180:32644/index.php
────────────────────────────────────────────────────────────────────────── Response number 5131: ───────────────────────────────────────────────────────────────────────────
 200       0 L       0 W         0 B  GET      http://94.237.59.180:32644/forum/
 RecursiveQueue:       Enqueued path http://94.237.59.180:32644/forum/ for recursion (rlevel=1, plugin_rlevel=0)
─────────────────────────────────────────────────────────────────────────── Response number 134: ───────────────────────────────────────────────────────────────────────────
 301       9 L      28 W       321 B  GET      http://94.237.59.180:32644/forum ➡ http://94.237.59.180:32644/forum/
 RedirectQueue:        Added a request to follow redirection
─────────────────────────────────────────────────────────────────────────── Response number 751: ───────────────────────────────────────────────────────────────────────────
 200       0 L       0 W         0 B  GET      http://94.237.59.180:32644/blog/
 RecursiveQueue:       Enqueued path http://94.237.59.180:32644/blog/ for recursion (rlevel=1, plugin_rlevel=0)
───────────────────────────────────────────────────────────────────────── Response number 259368: ──────────────────────────────────────────────────────────────────────────
 403       9 L      28 W       280 B  GET      http://94.237.59.180:32644/blog/.php
───────────────────────────────────────────────────────────────────────── Response number 350586: ──────────────────────────────────────────────────────────────────────────
 200      57 L      86 W      1046 B  GET      http://94.237.59.180:32644/blog/home.php
───────────────────────────────────────────────────────────────────────── Response number 350632: ──────────────────────────────────────────────────────────────────────────
 200       0 L       0 W         0 B  GET      http://94.237.59.180:32644/blog/index.php
───────────────────────────────────────────────────────────────────────── Response number 434697: ──────────────────────────────────────────────────────────────────────────
 403       9 L      28 W       280 B  GET      http://94.237.59.180:32644/forum/.php
───────────────────────────────────────────────────────────────────────── Response number 522667: ──────────────────────────────────────────────────────────────────────────
 200       0 L       1 W        21 B  GET      http://94.237.59.180:32644/forum/flag.php
───────────────────────────────────────────────────────────────────────── Response number 525961: ──────────────────────────────────────────────────────────────────────────
 200       0 L       0 W         0 B  GET      http://94.237.59.180:32644/forum/index.php
```
