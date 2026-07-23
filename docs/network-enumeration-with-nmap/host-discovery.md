# Host Discovery

## Ping Sweep

### Linux

```sh
my@attack:~$ for i in $(seq 254); do ping "10.129.2.$i" -c 1 -W 1 & done | grep from
```

### Windows

```pwsh
PS C:\Users\my> 1..254 | % {"10.129.2.$($_):$(Test-Connection -Count 1 -ComputerName 10.129.2.$($_) -Quiet)"} | Select-String True
```

## Scan Network Range

!!! failure

    Bazı güvenlik duvarları ICMP protokolünü engellemek yerine her pakete dönüş yapar. Bu durum tüm bilgisayarların ayakta olarak algılanmasına sebep olabilir.

    Aşağıdaki komut bu sorunu çözebilir:

    ```sh
    my@attack:~$ nmap 10.129.2.0/24 -sn --unprivileged
    ```

    Burada [unprivileged](https://nmap.org/book/man-misc-options.html) seçeneği kullanıcının RAW soket ayrıcalığına sahip olmadığını bildirir.

    Ek olarak aşağıdaki seçenekler de denenebilir:

    * -PS ([TCP SYN Ping](https://nmap.org/book/host-discovery-techniques.html))
    * -PA ([TCP ACK Ping](https://nmap.org/book/host-discovery-techniques.html))

```sh
my@attack:~$ sudo nmap 10.129.2.0/24 -sn | grep for | grep -oP '\d+\.\d+\.\d+\.\d+' > scope
```

| SCANNING OPTION | DESCRIPTION |
|---|---|
| 10.129.2.0/24 | Hedef ağ. |
| -sn | Port taramasını devre dışı bırak. |

## Scan IP List

```sh
my@attack:~$ sudo nmap -iL scope -sn
```

## Scan Multiple IP Addresses

```sh
my@attack:~$ sudo nmap -sn 10.129.2.47 10.129.2.48 10.129.2.49
```

## Scan Single IP

```sh
my@attack:~$ sudo nmap 10.129.2.47 -sn -PE --reason
```

| SCANNING OPTION | DESCRIPTION |
|---|---|
| 10.129.2.47 | Hedef adres. |
| -sn | Port taramasını devre dışı bırak. |
| -PE | ICMP yankı isteklerini aktif et. |
| --reason | Hedefin ayakta olma sebebini öğren. |
