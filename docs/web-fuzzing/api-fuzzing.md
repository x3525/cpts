# API Fuzzing

## Fuzzing

```sh
my@attack:~/webfuzz_api$ python3 api_fuzzer.py "http://94.237.59.180:32644" --wordlist /usr/local/share/wordlists/SecLists/Discovery/Web-Content/common.txt --output endpoints.txt
my@attack:~/webfuzz_api$ cat endpoints.txt
```

```output title="Output"
http://94.237.53.203:44692/czcmdcvt
http://94.237.53.203:44692/docs
```

## cURL

```sh
my@attack:~$ curl -s 'http://94.237.59.180:32644/czcmdcvt'
```

```json title="Output"
{"flag":"h1dd3n_r357"}
```
