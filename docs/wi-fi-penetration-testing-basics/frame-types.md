# Frame Types

## Beacon

```text title="Display Filter"
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 8)
```

| SOURCE | DESTINATION | PROTOCOL | INFO | TIME |
|---|---|---|---|---|
| Cisco-Li_82:b2:55 | Broadcast | 802.11 | Beacon frame, SN=3973, FN=0, Flags=........C, BI=100, SSID=Coherer | 0.000000 |

## Probe Request

```text title="Display Filter"
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 4)
```

| SOURCE | DESTINATION | PROTOCOL | INFO | TIME |
|---|---|---|---|---|
| Apple_82:36:3a | Broadcast | 802.11 | Probe Request, SN=1, FN=0, Flags=........C, BI=100, SSID=Coherer | 5.180060 |

## Probe Response

```text title="Display Filter"
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 5)
```

| SOURCE | DESTINATION | PROTOCOL | INFO | TIME |
|---|---|---|---|---|
| Cisco-Li_82:b2:55 | Apple_82:36:3a | 802.11 | Probe Response, SN=4031, FN=0, Flags=........C, BI=100, SSID=Coherer | 5.182047 |

## Authentication Request

```text title="Display Filter"
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 11)
```

| SOURCE | DESTINATION | PROTOCOL | INFO | TIME |
|---|---|---|---|---|
| Apple_82:36:3a | Cisco-Li_82:b2:55 | 802.11 | Authentication, SN=23, FN=0, Flags=........C | 5.645953 |

## Authentication Response

```text title="Display Filter"
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 11)
```

| SOURCE | DESTINATION | PROTOCOL | INFO | TIME |
|---|---|---|---|---|
| Cisco-Li_82:b2:55 | Apple_82:36:3a | 802.11 | Authentication, SN=4041, FN=0, Flags=........C | 5.645953 |

## Association Request

```text title="Display Filter"
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 0)
```

| SOURCE | DESTINATION | PROTOCOL | INFO | TIME |
|---|---|---|---|---|
| Apple_82:36:3a | Cisco-Li_82:b2:55 | 802.11 | Association Request, SN=24, FN=0, Flags=........C, SSID=Coherer | 5.645953 |

## Association Response

```text title="Display Filter"
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 1)
```

| SOURCE | DESTINATION | PROTOCOL | INFO | TIME |
|---|---|---|---|---|
| Cisco-Li_82:b2:55 | Apple_82:36:3a | 802.11 | Association Response, SN=4042, FN=0, Flags=........C, SSID=Coherer | 5.647953 |

## Disassociation and Deauthentication

```text title="Display Filter"
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 10) || (wlan.fc.type_subtype == 12)
```

| SOURCE | DESTINATION | PROTOCOL | INFO | TIME |
|---|---|---|---|---|
| Apple_82:36:3a | Cisco-Li_82:b2:55 | 802.11 | Disassociate, SN=181, FN=0, Flags=........C | 36.799791 |
