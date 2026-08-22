# Domain Information

## [Certificate Transparency](https://crt.sh/)

```sh
my@attack:~$ curl -s 'https://crt.sh/json?q=inlanefreight.com' | jq .
```

## Subdomains

```sh
my@attack:~$ curl -s 'https://crt.sh/json?q=inlanefreight.com' | jq -r '.[] | select(.name_value) | .name_value' | sort -u | tee sub.txt
```

```output title="Output"
inlanefreight.com
www.inlanefreight.com
```

## Company Hosted Servers

!!! warning

    Şirket ismi belirtilerek hosting sağlayan üçüncü parti sağlayıcılar elendi.

```sh
my@attack:~$ while IFS= read -r name; do host "$name" | grep 'has address' | grep inlanefreight.com; done < sub.txt
```

```output title="Output"
inlanefreight.com has address 134.209.24.248
www.inlanefreight.com has address 134.209.24.248
```

## Target Address List

```sh
my@attack:~$ while IFS= read -r name; do host "$name" | grep 'has address' | grep inlanefreight.com; done < sub.txt | cut -d ' ' -f 4 | sort -u | tee ip.txt
```

```output title="Output"
134.209.24.248
```

## Shodan

```sh
my@attack:~$ while IFS= read -r i; do shodan host "$i"; done < ip.txt
```

## DNS Records

```sh
my@attack:~$ dig ANY inlanefreight.com
```
