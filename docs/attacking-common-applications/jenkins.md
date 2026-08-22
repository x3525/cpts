# Jenkins

## Default Credentials

* admin
* admin

## Version

```sh
my@attack:~$ curl -s 'http://jenkins.inlanefreight.local:8000' -H 'Cookie: JSESSIONID.d17ec385=node0fw96f6s5oh3pvsdxphbkhmgp4.node0' | html2text | grep -P 'Jenkins [0-9]'
```

```output title="Output"
[Jenkins 2.303.1](https://www.jenkins.io/)
```
