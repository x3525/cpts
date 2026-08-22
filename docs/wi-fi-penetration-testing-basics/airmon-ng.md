# Airmon-ng

## Listing Wireless Devices

```sh
my@attack:~$ sudo airmon-ng
```

```output title="Output"
PHY     Interface       Driver          Chipset

phy0    wlp0s20f0u3     ath9k_htc       Qualcomm Atheros Communications AR9271 802.11n
```

## Starting Monitor Mode

```sh
my@attack:~$ sudo airmon-ng start wlp0s20f0u3
```

```output title="Output" hl_lines="4"
PHY     Interface       Driver          Chipset

phy0    wlp0s20f0u3     ath9k_htc       Qualcomm Atheros Communications AR9271 802.11n
                (mac80211 monitor mode vif enabled for [phy0]wlp0s20f0u3 on [phy0]wlp0s20f0u3mon)
                (mac80211 station mode vif disabled for [phy0]wlp0s20f0u3)
```

## Checking the Mode

```sh
my@attack:~$ iwconfig
```

```output title="Output" hl_lines="3"
lo        no wireless extensions.

wlp0s20f0u3mon  IEEE 802.11  Mode:Monitor  Frequency:2.412 GHz  Tx-Power=20 dBm
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:off
```

## Checking Interfering Processes

```sh
my@attack:~$ sudo airmon-ng check
```

```output title="Output" hl_lines="7-8"
Found 2 processes that could cause trouble.
Kill them using 'airmon-ng check kill' before putting
the card in monitor mode, they will interfere by changing channels
and sometimes putting the interface back in managed mode

    PID Name
    487 NetworkManager
    527 wpa_supplicant
```

## Killing Processes

```sh
my@attack:~$ sudo airmon-ng check kill
```

```output title="Output"
Killing these processes:

    PID Name
   6109 wpa_supplicant
```

## Stopping Monitor Mode

```sh
my@attack:~$ sudo airmon-ng stop wlp0s20f0u3mon
```

```output title="Output" hl_lines="5"
PHY     Interface       Driver          Chipset

phy0    wlp0s20f0u3mon  ath9k_htc       Qualcomm Atheros Communications AR9271 802.11n
                (mac80211 station mode vif enabled on [phy0]wlp0s20f0u3)
                (mac80211 monitor mode vif disabled for [phy0]wlp0s20f0u3mon)
```
