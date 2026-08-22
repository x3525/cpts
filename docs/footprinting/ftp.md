# FTP

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 20 | File Transfer Protocol (FTP) data transfer |
| 21 | File Transfer Protocol (FTP) control (command) |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.202.5 -p 21 -sV -sC
```

## Dangerous Settings

| OPTION | VALUE |
|---|---|
| anon_mkdir_write_enable | yes |
| anon_upload_enable | yes |
| anonymous_enable | yes |
| no_anon_password | yes |
| write_enable | yes |

## Service Interaction

### Netcat

```sh
my@attack:~$ nc -vn 10.129.202.5 21
```

### OpenSSL

```sh
my@attack:~$ openssl s_client -connect 10.129.202.5:21 -starttls ftp
```

## Anonymous Login

```sh
my@attack:~$ ftp 10.129.202.5
```

## Active Mode

!!! failure

    Güvenlik duvarı, istemciye gelen bağlantıları engelliyor ise veri aktarım kanalı kurulamaz.

    Pasif mod ile bağlanmak için aşağıdaki komutu kullan:

    ```sh
    my@attack:~$ ftp 10.129.202.5 -p
    ```

    Ya da bağlantı kurulduktan sonra:

    ```sh
    ftp> pass
    ```

| CHANNEL | PORT | CONNECTION | DESCRIPTION |
|---|---|---|---|
| Command | 21 | Client --> Server | Bu kanal istemci tarafından kurulur. |
| Data | 20 | Client <-- Server | Bu kanal sunucu tarafından kurulur. |

## Passive Mode

| CHANNEL | PORT | CONNECTION | DESCRIPTION |
|---|---|---|---|
| Command | 21 | Client --> Server | Bu kanal istemci tarafından kurulur. |
| Data | (P1 * 256) + P2 | Client --> Server | Bu kanal istemci tarafından kurulur. |

## Passive Mode Example

!!! warning

    Netcat üzerinde girilen komutlar ++enter++ ile gönderilir ise sadece \\n gönderilir.

    FTP ise \\r\\n bekler.

    Bunun için her komut ardından bas:

    1. ++control+v++
    2. ++enter++
    3. ++enter++

### Connection

```sh
my@attack:~$ nc -vn 10.129.202.5 21
```

```output title="Output"
Connection to 10.129.202.5 21 port [tcp/*] succeeded!
220 Microsoft FTP Service
```

### Login

```sh
USER anonymous
PASS password
```

### Data Channel ([LIST](https://en.wikipedia.org/wiki/List_of_FTP_commands))

Öncelikle pasif mod aktif edilir:

```sh
PASV
```

```output title="Output"
227 Entering Passive Mode (10,129,202,5,194,11).
```

Veri kanalı port numarası hesaplanır:

```text title="Formula"
(P1 * 256) + P2 = (194 * 256) + 11 = 49675
```

Veri kanalı bağlantısı için aşağıda verilen komut çalıştırılır:

```sh
my@attack:~$ nc -vn 10.129.202.5 49675
```

```output title="Output"
Connection to 10.129.202.5 49675 port [tcp/*] succeeded!
```

Daha önceden açılan komut kanalı (21) üzerinde aşağıdaki komut kullanılır:

```sh
LIST
```

```output title="Output"
125 Data connection already open; Transfer starting.
226 Transfer complete.
```

Sonuçlar, veri kanalı tarafında görülür:

```output title="Output"
02-08-25  09:37PM                  438 Note-From-IT.txt
```

### Data Channel ([RETR](https://en.wikipedia.org/wiki/List_of_FTP_commands))

Öncelikle pasif mod aktif edilir:

```sh
PASV
```

```output title="Output"
227 Entering Passive Mode (10,129,202,5,194,12).
```

Veri kanalı port numarası hesaplanır:

```text title="Formula"
(P1 * 256) + P2 = (194 * 256) + 12 = 49676
```

Veri kanalı bağlantısı için aşağıda verilen komut çalıştırılır:

```sh
my@attack:~$ nc -vn 10.129.202.5 49676
```

```output title="Output"
Connection to 10.129.202.5 49676 port [tcp/*] succeeded!
```

Daha önceden açılan komut kanalı (21) üzerinde aşağıdaki komut kullanılır:

```sh
RETR Note-From-IT.txt
```

```output title="Output"
125 Data connection already open; Transfer starting.
226 Transfer complete.
```

Sonuçlar, veri kanalı tarafında görülür:

```output title="Output"
Bertolis,

The website is still under construction. To stop users from poking their nose where it doesn't belong, I've configured IIS to only allow requests containing a specific user-agent header. If you'd like to test it out, please provide the following header to your HTTP request.

User-Agent: Server Administrator

The site should be finished within the next couple of weeks. I'll keep you posted.

Cheers,
jarednexgent
```

## FTPUSERS

!!! warning

    Bu dosyada adı geçen kullanıcılar FTP servisini kullanamaz.

```sh
my@attack:~$ cat /etc/ftpusers
```

```output title="Output"
guest
john
kevin
```

## Status

```sh
ftp> status
```

## Recursive Listing

```sh
ftp> ls -R
```

## Executing System Commands

```sh
ftp> !cat flag.txt
```

## Download Operations

### Download a File

```sh
ftp> get flag.txt
```

### Download Multiple Files

```sh
ftp> mget flag.txt test.txt
```

### Download All Available Files

```sh
my@attack:~$ wget -q 'ftp://anonymous:anonymous@10.129.202.5' --mirror --no-passive-ftp
```

## Upload Operations

### Upload a File

```sh
ftp> put proof.txt
```

### Upload Multiple Files

```sh
ftp> mput proof.txt poc.txt
```
