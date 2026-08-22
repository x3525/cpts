# Creating Custom Wordlists

## Previously Cracked Passwords

```sh
my@attack:~$ cat ~/.local/share/hashcat/hashcat.potfile
```

## Password Mutations

```sh
my@attack:~$ hashcat Wordlist -r RuleFile --stdout
```

## [CUPP](https://github.com/Mebus/cupp)

```sh
my@attack:~$ cupp.py -i
```

## [CeWL](https://github.com/digininja/CeWL)

```sh
my@attack:~$ cewl URL -d Depth -m MinWordLength -w OutputFile
```

## KwProcessor

```sh
my@attack:~$ kwp -s 1 basechars/full.base keymaps/en-us.keymap routes/2-to-10-max-3-direction-changes.route
```

## PrinceProcessor

```sh
my@attack:~$ pp64.bin --elem-cnt-min=3 -o wordlist.txt < words
```

## Example

### Saving a Web Page

1. Firefox
2. marvel.com
    * Open application menu
    * Save page as: index.html

### Web Server

```sh
my@attack:~$ python3 -m http.server 8000
```

### Generating the Wordlist

```sh
my@attack:~$ cewl http://localhost:8000 -w wordlist.txt
```

### [Munge](https://github.com/Th3S3cr3tAg3nt/Munge)

```sh
my@attack:~$ munge3.py -l 3 -i wordlist.txt -o wordlist_munged.txt
```

### [OneRuleToRuleThemAll](https://github.com/NotSoSecure/password_cracking_rules/blob/master/OneRuleToRuleThemAll.rule)

```sh
my@attack:~$ hashcat -a 0 -m 0 df494bf816de8291d42089664447b887 wordlist_munged.txt -r /usr/local/share/wordlists/OneRuleToRuleThemAll.rule
```
