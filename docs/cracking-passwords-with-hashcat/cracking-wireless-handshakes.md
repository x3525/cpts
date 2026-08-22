# Cracking Wireless Handshakes

## MIC

### [Converting](https://hashcat.net/cap2hashcat/)

```sh
my@attack:~$ cap2hccapx CapFile HashFile
```

### Cracking the Handshakes

=== "NEW"

    ```sh
    my@attack:~$ hashcat -a 0 -m 22000 HashFile Wordlist
    ```

=== "OLD"

    ```sh
    my@attack:~$ hashcat -a 0 -m 2500 HashFile Wordlist --deprecated-check-disable
    ```

## PMKID

### Extracting

=== "NEW"

    ```sh
    my@attack:~$ hcxpcapngtool CapFile -o HashFile
    ```

=== "OLD"

    ```sh
    my@attack:~$ hcxpcaptool CapFile -z HashFile
    ```

### Cracking the Hash

=== "NEW"

    ```sh
    my@attack:~$ hashcat -a 0 -m 22000 HashFile Wordlist
    ```

=== "OLD"

    ```sh
    my@attack:~$ hashcat -a 0 -m 16800 HashFile Wordlist --deprecated-check-disable
    ```
