# Working with Rules

## [Rules](https://hashcat.net/wiki/doku.php?id=rule_based_attack)

```sh
my@attack:~$ hashcat -a 0 -m HashType Hash Wordlist -r RuleFile -r RuleFile
```

| FUNCTION | DESCRIPTION |
|---|---|
| : | Do nothing (passthrough) |
| l | Lowercase all letters |
| u | Uppercase all letters |
| c | Capitalize the first character, lowercase the rest |
| C | Lowercase the first character, uppercase the rest |
| t | Toggle the case of all characters in word |
| TN | Toggle the case of characters at position N |
| r | Reverse the entire word |
| d | Duplicate entire word |
| pN | Append duplicated word N times |
| f | Duplicate word reversed |
| { | Rotate the word left |
| } | Rotate the word right |
| $X | Append character X to end |
| ^X | Prepend character X to front |
| \[ | Delete first character |
| \] | Delete last character |
| DN | Delete character at position N |
| xNM | Extract M characters, starting at position N |
| ONM | Delete M characters, starting at position N |
| iNX | Insert character X at position N |
| oNX | Overwrite character at position N with X |
| 'N | Truncate word at position N |
| sXY | Replace all instances of X with Y |
| @X | Purge all instances of X |
| zN | Duplicate first character N times |
| ZN | Duplicate last character N times |
| q | Duplicate every character |
| XNMI | Insert substring of length M starting from position N of word saved to memory at position I |
| 4 | Append the word saved to memory to current word |
| 6 | Prepend the word saved to memory to current word |
| M | Memorize current word |

## Rules ([Random](https://hashcat.net/wiki/doku.php?id=rule_based_attack#random_rules))

```sh
my@attack:~$ hashcat -a 0 -m HashType Hash Wordlist -g NumberOfRules
```

## Rules (Collection)

```sh
my@attack:~$ ls /usr/share/doc/hashcat/rules
```
