# Airdecap-ng

## Decrypting Unencrypted Captures

| OPTION | DESCRIPTION |
|---|---|
| -b | Erişim noktasına ait MAC adresi. |

```sh
my@attack:~$ sudo airdecap-ng -b 00:14:6C:7A:41:81 open_capture.cap
```

```output title="Output"
Total number of stations seen            0
Total number of packets read           251
Total number of WEP data packets         0
Total number of WPA data packets         0
Number of plaintext data packets         0
Number of decrypted WEP  packets         0
Number of corrupted WEP  packets         0
Number of decrypted WPA  packets         0
Number of bad TKIP (WPA) packets         0
Number of bad CCMP (WPA) packets         0
```

## Decrypting WEP Encrypted Captures

| OPTION | DESCRIPTION |
|---|---|
| -w | Hedef ağa ait hexadecimal WEP anahtarı. |

```sh
my@attack:~$ sudo airdecap-ng -w 11A3E229084349BC25D97E2939 capture.cap
```

```output title="Output" hl_lines="3"
Total number of stations seen            6
Total number of packets read           356
Total number of WEP data packets       235
Total number of WPA data packets       121
Number of plaintext data packets         0
Number of decrypted WEP  packets         0
Number of corrupted WEP  packets         0
Number of decrypted WPA  packets       235
Number of bad TKIP (WPA) packets         0
Number of bad CCMP (WPA) packets         0
```

## Decrypting WPA Encrypted Captures

| OPTION | DESCRIPTION |
|---|---|
| -e | Hedef ağa ait SSID bilgisi. |
| -p | Hedef ağa ait WPA geçiş ifadesi. |

```sh
my@attack:~$ sudo airdecap-ng -e 'CyberNet-Secure' -p 'HTB_@cademy' capture.cap
```

```output title="Output" hl_lines="4"
Total number of stations seen            6
Total number of packets read           356
Total number of WEP data packets       235
Total number of WPA data packets       121
Number of plaintext data packets         0
Number of decrypted WEP  packets         0
Number of corrupted WEP  packets         0
Number of decrypted WPA  packets       121
Number of bad TKIP (WPA) packets         0
Number of bad CCMP (WPA) packets         0
```
