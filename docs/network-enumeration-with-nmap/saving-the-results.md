# Saving the Results

## All Formats

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p- -oA results
```

## Normal Output

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p- -oN results.nmap
```

## XML Output

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p- -oX results.xml
```

## Grepable Output

```sh
my@attack:~$ sudo nmap 10.129.2.47 -p- -oG results.gnmap
```

## Style Sheets

```sh
my@attack:~$ xsltproc results.xml -o results.html
```
