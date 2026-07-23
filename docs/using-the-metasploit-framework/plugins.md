# Plugins

## Find Plugins Directory

```sh
my@attack:~$ find / -wholename "*metasploit*plugins*" -type d 2> /dev/null
```

## Download Plugins

```sh
my@attack:~$ git clone https://github.com/darkoperator/Metasploit-Plugins.git
```

## Copy Plugin

```sh
my@attack:~$ sudo cp Metasploit-Plugins/pentest.rb /opt/metasploit/plugins
```

## Load Plugin

```sh
msf6 > load pentest
```
