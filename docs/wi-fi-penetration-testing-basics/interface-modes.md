# Interface Modes

## Current Mode

```sh
my@attack:~$ iwconfig
```

```output title="Output" hl_lines="4"
lo        no wireless extensions.

wlp0s20f0u3  IEEE 802.11  ESSID:off/any
          Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:off
```

## Managed

```sh
my@attack:~$ sudo ifconfig wlp0s20f0u3 down
my@attack:~$ sudo iwconfig wlp0s20f0u3 mode managed
my@attack:~$ sudo ifconfig wlp0s20f0u3 up
```

## Monitor

```sh
my@attack:~$ sudo ifconfig wlp0s20f0u3 down
my@attack:~$ sudo iwconfig wlp0s20f0u3 mode monitor
my@attack:~$ sudo ifconfig wlp0s20f0u3 up
```

## AD-HOC

```sh
my@attack:~$ sudo ifconfig wlp0s20f0u3 down
my@attack:~$ sudo iwconfig wlp0s20f0u3 mode ad-hoc
my@attack:~$ sudo ifconfig wlp0s20f0u3 up
```

## Master

### Management Daemon Configuration

```ini title="open.conf" linenums="1" hl_lines="3"
interface=wlp0s20f0u3
driver=nl80211
ssid=Hello-World
channel=1
hw_mode=g
```

### Activating Access Point

```sh
my@attack:~$ sudo hostapd open.conf
```

```output title="Output"
wlp0s20f0u3: interface state UNINITIALIZED->ENABLED
wlp0s20f0u3: AP-ENABLED
wlp0s20f0u3: STA 2c:6d:c1:af:eb:91 IEEE 802.11: authenticated
wlp0s20f0u3: STA 2c:6d:c1:af:eb:91 IEEE 802.11: associated (aid 1)
wlp0s20f0u3: AP-STA-CONNECTED 2c:6d:c1:af:eb:91
wlp0s20f0u3: STA 2c:6d:c1:af:eb:91 RADIUS: starting accounting session DAE495B9EB2ED356
```
