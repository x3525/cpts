# Performance

## Optimized RTT

!!! warning

    Hızlı tarama bazı hedefleri kaçırabilir.

```sh
my@attack:~$ sudo nmap 10.129.2.0/24 --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

| SCANNING OPTION | DESCRIPTION |
|---|---|
| --initial-rtt-timeout | Başlangıç RTT zaman aşımını ayarla. |
| --max-rtt-timeout | Maksimum RTT zaman aşımını ayarla. |

## Max Retries

!!! warning

    Sonuçlar üzerinde olumsuz bir etkiye sebep olabilir.

```sh
my@attack:~$ sudo nmap 10.129.2.0/24 --max-retries 0
```

## Rates

!!! warning

    Oran ne kadar yüksek ise sonuçlar o kadar güvenilmez olur.

```sh
my@attack:~$ sudo nmap 10.129.2.0/24 --min-rate 300
```

## [Timing Templates](https://nmap.org/book/performance-timing-templates.html)

```sh
my@attack:~$ sudo nmap 10.129.2.0/24 -T5
```

| T0 | T1 | T2 | T3 | T4 | T5 |
|---|---|---|---|---|---|
| Paranoid | Sneaky | Polite | Normal | Aggressive | Insane |
