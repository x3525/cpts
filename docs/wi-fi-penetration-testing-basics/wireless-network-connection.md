# Wireless Network Connection

## Configuration File

### WEP

```nginx title="network.conf" linenums="1"
network={
    ssid="HackTheBox-WEP"
    key_mgmt=NONE
    wep_key0=1A2B3C4D5E
    wep_tx_keyidx=0
}
```

### WPA

```nginx title="network.conf" linenums="1"
network={
    ssid="CyberNet-Secure"
    psk="HTB_@cademy"
}
```

### WPA Enterprise

```nginx title="network.conf" linenums="1"
network={
    ssid="HTB-Corp"
    key_mgmt=WPA-EAP
    identity="HTB\Sentinal"
    password="sentinal"
}
```

## Connecting

```sh
my@attack:~$ sudo wpa_supplicant -c network.conf -i wlp0s20f0u3
```

## Obtaining an Address

```sh
my@attack:~$ sudo dhclient wlp0s20f0u3 -r
my@attack:~$ sudo dhclient wlp0s20f0u3
```
