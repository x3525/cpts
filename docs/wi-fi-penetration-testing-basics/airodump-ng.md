# Airodump-ng

| FIELD | DESCRIPTION |
|---|---|
| BSSID | Erişim noktasına ait MAC adresi. |
| PWR | Ağ gücü. |
| Beacons | Ağ tarafından gönderilen duyuru paketlerinin miktarı. |
| #Data | Yakalanan veri paketi miktarı. |
| #/s | Son 10 saniyede yakalanan veri paketi miktarı. |
| CH | Ağ kanalı. |
| MB | Ağ tarafından desteklenen maksimum hız. |
| ENC | Ağ tarafından kullanılan şifreleme yöntemi. |
| CIPHER | Ağ tarafından kullanılan şifre. |
| AUTH | Ağ tarafından kullanılan kimlik doğrulama yöntemi. |
| ESSID | Ağ adı. |
| STATION | Ağa bağlı istemciye ait MAC adresi. |
| Rate | Erişim noktası ve istemci arasındaki veri aktarım hızı. |
| Lost | Kaybedilen paket miktarı. |
| Frames | İstemci tarafından gönderilen çerçeve miktarı. |
| Notes | İstemci hakkında ek bilgiler. |
| Probes | İstemci tarafından taranan ağlar. |

## Scanning

```sh
my@attack:~$ sudo airodump-ng wlp0s20f0u3mon
```

```output title="Output" hl_lines="5-6"
 CH 12 ][ Elapsed: 1 min ][ 2025-04-21 07:55

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 D8:D6:3A:EB:29:D4  -28       73        0    0  11   54   WPA2 CCMP   PSK  HackTheBox
 D8:D6:3D:EB:29:D5  -47       77       30    0   1   54   WPA2 CCMP   PSK  CyberNet-Secure

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 D8:D6:3D:EB:29:D5  EE:97:33:98:DC:ED  -29    0 - 1      0       12
 D8:D6:3D:EB:29:D5  96:48:DC:19:DF:3C  -29    0 - 1      0       16
 D8:D6:3D:EB:29:D5  3A:0C:3B:5F:9F:42  -29    1 - 1      0       16
 D8:D6:3D:EB:29:D5  E2:F6:D7:AC:C3:45  -29    0 - 1      0       23
```

## Selecting Channels

```sh
my@attack:~$ sudo airodump-ng wlp0s20f0u3mon --channel 6,11
```

```output title="Output" hl_lines="5"
 CH 11 ][ Elapsed: 1 min ][ 2025-04-21 07:56

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 D8:D6:3A:EB:29:D4  -28      875        0    0  11   54   WPA2 CCMP   PSK  HackTheBox

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 (not associated)   EE:97:33:98:DC:ED  -29    0 - 1     56        9
 (not associated)   96:48:DC:19:DF:3C  -29    0 - 1     57       11
 (not associated)   3A:0C:3B:5F:9F:42  -29    0 - 1      0       23
 (not associated)   E2:F6:D7:AC:C3:45  -29    0 - 1      0       21
```

## Selecting Bands

| LETTER | FREQUENCY |
|---|---|
| a | 5 GHz |
| b | 2.4 GHz |
| g | 2.4 GHz |

```sh
my@attack:~$ sudo airodump-ng wlp0s20f0u3mon --band ab
```

```output title="Output" hl_lines="5-6"
 CH 56 ][ Elapsed: 1 min ][ 2025-04-21 07:57

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 D8:D6:3A:EB:29:D4  -28       58        0    0  48   54   WPA2 CCMP   PSK  HackTheBox-5G
 D8:D6:3D:EB:29:D5  -47       19       10    0   1   54   WPA2 CCMP   PSK  CyberNet-Secure

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 D8:D6:3D:EB:29:D5  EE:97:33:98:DC:ED  -29    0 - 1      0        5
 D8:D6:3D:EB:29:D5  96:48:DC:19:DF:3C  -29    0 - 1      0        3
 D8:D6:3D:EB:29:D5  3A:0C:3B:5F:9F:42  -29    0 - 1      0       12
 D8:D6:3D:EB:29:D5  E2:F6:D7:AC:C3:45  -29    0 - 1     79       16
```
