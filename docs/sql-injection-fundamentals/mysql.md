# MySQL

## CLI

### Password Prompt

```sh
my@attack:~$ sudo mariadb -h 94.237.53.113 -P 41667 -u root -p
```

```sh
my@attack:~$ sudo mariadb -h 94.237.53.113 -P 41667 -u root -p --skip-ssl
```

### Password in Command

!!! warning

    Parola ile -p seçeneği arasında boşluk olmamalıdır.

```sh
my@attack:~$ sudo mariadb -h 94.237.53.113 -P 41667 -u root -p'password'
```

## Databases

### Creating a Database

```mysql
MariaDB [(none)]> CREATE DATABASE users;
```

### Show Databases

```mysql
MariaDB [(none)]> SHOW DATABASES;
```

```output title="Output"
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
| users              |
+--------------------+
6 rows in set (0.002 sec)
```

### Using a Database

```mysql
MariaDB [(none)]> USE users;
```

```output title="Output"
Database changed
```

## Tables

### Creating a Table

```mysql
MariaDB [users]> CREATE TABLE logins (id INT NOT NULL AUTO_INCREMENT, username VARCHAR(100) UNIQUE NOT NULL, password VARCHAR(100) NOT NULL, date_of_joining DATETIME DEFAULT NOW(), PRIMARY KEY (id));
```

### Show Tables

```mysql
MariaDB [users]> SHOW TABLES;
```

```output title="Output"
+-----------------+
| Tables_in_users |
+-----------------+
| logins          |
+-----------------+
1 row in set (0.001 sec)
```

### Describing a Table

```mysql
MariaDB [users]> DESCRIBE logins;
```

```output title="Output"
+-----------------+--------------+------+-----+---------------------+----------------+
| Field           | Type         | Null | Key | Default             | Extra          |
+-----------------+--------------+------+-----+---------------------+----------------+
| id              | int(11)      | NO   | PRI | NULL                | auto_increment |
| username        | varchar(100) | NO   | UNI | NULL                |                |
| password        | varchar(100) | NO   |     | NULL                |                |
| date_of_joining | datetime     | YES  |     | current_timestamp() |                |
+-----------------+--------------+------+-----+---------------------+----------------+
4 rows in set (0.002 sec)
```
