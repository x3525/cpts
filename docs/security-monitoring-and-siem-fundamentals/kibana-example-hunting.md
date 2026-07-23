# Kibana Example Hunting

## Indicators of Compromise

### OneNote File

* [https://transfer.sh/get/kNxU7/invoice.one](https://transfer.sh/get/kNxU7/invoice.one)
* [https://mega.io/dl9o1Dz/invoice.one](https://mega.io/dl9o1Dz/invoice.one)

### PowerShell Script

* [https://pastebin.com/raw/AvHtdKb2](https://pastebin.com/raw/AvHtdKb2)
* [https://pastebin.com/raw/gj58DKz](https://pastebin.com/raw/gj58DKz)

### C2 Nodes

* 91.90.213.14:443
* 103.248.70.64:443
* 141.98.6.59:443

### File Hashes

* 226A723FFB4A91D9950A8B266167C5B354AB0DB1DC225578494917FE53867EF2
* C346077DAD0342592DB753FE2AB36D2F9F1C76E55CF8556FE5CDA92897E99C7E
* 018D37CBD3878258C29DB3BC3F2988B6AE688843801B9ABC28E6151141AB66D4

## Date Range

Last 15 years

## Index Pattern

windows*

## Timezone for Date Formatting

Europe/Copenhagen

## The Hunt

Senaryo, bir oltalama e-postası üzerinden indirilen bir OneNote dosyasını temel almaktadır.

Öncelikle, [Sysmon Event ID 15](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90015) (FileCreateStreamHash) olayını araştır.

```kql title="Query"
event.code: 15 AND file.name: *invoice.one
```

| FIELD | VALUE |
|---|---|
| @timestamp | Mar 26, 2023 @ 22:05:47.793 |
| [file](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_file).directory | C:\Users\bob\Downloads |
| [process](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_process).name | msedge.exe |

Herhangi bir tarayıcı dahil olmadan gerçekleşen dosya indirme olaylarını yakalamak için [Sysmon Event ID 11](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90011) (FileCreate) olayını araştır.

Sorgu, internet üzerinden indirilen dosyaları tespit etmek için [Zone.Identifier](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-fscc/6e3f7352-d11c-4d76-8c39-2516a9df36e8) Alternate Data Streams ([ADS](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-fscc/e2b19412-a925-4360-b009-86e3b8a020c8)) içeren dosya adlarını arar.

```kql title="Query"
event.code: 11 AND file.name: invoice.one*
```

| FIELD | VALUE |
|---|---|
| @timestamp | Mar 26, 2023 @ 22:05:47.789 |
| [file](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_file).directory | C:\Users\bob\Downloads |
| [file](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_file).name | invoice.one:Zone.Identifier |
| [host](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_host).hostname | WS001 |

WS001 bilgisayarına ait IP adresini öğrenmek için [Sysmon Event ID 3](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90003) (Network connection detected) olayını araştır.

```kql title="Query"
event.code: 3 AND host.hostname: WS001
```

| FIELD | VALUE |
|---|---|
| [host](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_host).hostname | WS001 |
| [source](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_source).ip | 192.168.28.130 |

WS001 bilgisayarı tarafından üretilen DNS sorgularını araştır.

Dosyanın indirildiği zaman aralığını kullan:

1. Mar 26, 2023 @ 22:05:00.000
2. Mar 26, 2023 @ 22:05:48.000

```kql title="Query"
source.ip: 192.168.28.130 AND dns.question.name: *
```

Sorgu boş sonuç döndürebilir. Çünkü varsayılan Sysmon yapılandırması, tarayıcılar tarafından üretilen ağ bağlantılarını toplamaz.

Geçerli indeks [zeek](https://zeek.org/)* olarak değiştirilir, sorgu tekrarlanır.

```kql title="Query"
source.ip: 192.168.28.130 AND dns.question.name: *
```

Sorgu, gürültü içeren sonuçları da getirir.

Gürültü içeren sonuçları kaldırmak için [dns](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_dns).question.name üzerinde en çok tekrar eden değerleri (Top 5 values) filtrele.

* NOT dns.question.name:*
* NOT dns.question.name:www.google-analytics.com
* NOT dns.question.name:30.2.52.216.in-addr.arpa
* NOT dns.question.name:www.google.com

!!! tip

    Elde edilen sonuçları daha kolay analiz etmek için [dns](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_dns).question.name sütun olarak eklenebilir (Add field as column).

| TIME | DNS.QUESTION.NAME |
|---|---|
| Mar 26, 2023 @ 22:05:35.604 | nav-edge.smartscreen.microsoft.com |
| Mar 26, 2023 @ 22:05:35.603 | nav-edge.smartscreen.microsoft.com |
| Mar 26, 2023 @ 22:05:35.548 | file.io |
| Mar 26, 2023 @ 22:05:35.548 | file.io |
| Mar 26, 2023 @ 22:05:35.541 | file.io |
| Mar 26, 2023 @ 22:05:02.703 | mail.google.com |
| Mar 26, 2023 @ 22:05:02.702 | mail.google.com |

Gerçekleşen olaylar (tablo tersten okundu):

1. Gmail adresine erişilmiş.
2. Başka bir siteye ([file.io](https://www.file.io/)) bağlantı kurulmuş.
3. Microsoft Defender SmartScreen dosya taraması başlamış.

[file.io](https://www.file.io/) sitesine ait IP adresini öğren:

| FIELD | VALUE |
|---|---|
| [dns](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_dns).answers.data | 34.197.10.85, 3.213.216.16 |
| [dns](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_dns).question.name | file.io |

Siteye kurulan bağlantıları araştır.

```kql title="Query"
"34.197.10.85"
```

!!! tip

    Elde edilen sonuçları daha kolay analiz etmek için [source](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_source).ip ve [destination](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_destination).ip sütun olarak eklenebilir (Add field as column).

| TIME | SOURCE.IP | DESTINATION.IP |
|---|---|---|
| Mar 26, 2023 @ 22:05:41.905 | 192.168.28.130 | 34.197.10.85 |
| Mar 26, 2023 @ 22:05:38.538 | 192.168.28.130 | 34.197.10.85 |
| Mar 26, 2023 @ 22:05:38.435 | 192.168.28.130 | 34.197.10.85 |
| Mar 26, 2023 @ 22:05:35.710 | 192.168.28.130 | 34.197.10.85 |
| Mar 26, 2023 @ 22:05:35.594 | 192.168.28.130 | 34.197.10.85 |

İndirilen dosyaya erişilip/erişilmediğini öğrenmek için [Sysmon Event ID 1](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90001) (Process creation) olayını araştır.

Ancak bundan önce:

* Zaman aralığı Last 15 years yapılır.
* Geçerli indeks windows* yapılır.
* Sütun olarak eklenen tüm alanlar kaldırılır.

```kql title="Query"
event.code: 1 AND process.command_line: *invoice.one*
```

| FIELD | VALUE |
|---|---|
| @timestamp | Mar 26, 2023 @ 22:05:53.601 |
| [process](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_process).name | ONENOTE.EXE |

Kullanıcı, dosyayı indirdikten yaklaşık 6 saniye sonra dosyaya erişmiş.

Ebeveyn işlem ONENOTE.EXE olacak şekilde [Sysmon Event ID 1](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90001) (Process creation) olayını araştır.

```kql title="Query"
event.code: 1 AND process.parent.name: ONENOTE.EXE
```

| FIELD | VALUE |
|---|---|
| [process](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_process).name | cmd.exe |
| [process](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs#_process).parent.name | ONENOTE.EXE |
