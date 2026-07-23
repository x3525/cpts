# SQL Statements

## INSERT Statement

### INSERT All

```mysql
MariaDB [users]> INSERT INTO logins VALUES (1, 'admin', 'p@ssw0rd', '2020-07-02');
```

### INSERT Specific

```mysql
MariaDB [users]> INSERT INTO logins (username, password) VALUES ('administrator', 'adm1n_p@ss');
```

### INSERT Multiple

```mysql
MariaDB [users]> INSERT INTO logins (username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!');
```

## SELECT Statement

### SELECT All

```mysql
MariaDB [users]> SELECT * FROM logins;
```

```output title="Output"
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  2 | administrator | adm1n_p@ss | 2024-11-10 09:52:57 |
|  3 | john          | john123!   | 2024-11-10 09:53:05 |
|  4 | tom           | tom123!    | 2024-11-10 09:53:05 |
+----+---------------+------------+---------------------+
4 rows in set (0.001 sec)
```

### SELECT Specific

```mysql
MariaDB [users]> SELECT username,password FROM logins;
```

```output title="Output"
+---------------+------------+
| username      | password   |
+---------------+------------+
| admin         | p@ssw0rd   |
| administrator | adm1n_p@ss |
| john          | john123!   |
| tom           | tom123!    |
+---------------+------------+
4 rows in set (0.001 sec)
```

## UPDATE Statement

```mysql
MariaDB [users]> UPDATE logins SET password = '1234' WHERE id > 1;
```

## ALTER Statement

### DROP

```mysql
MariaDB [users]> ALTER TABLE logins DROP old;
```

### ADD

```mysql
MariaDB [users]> ALTER TABLE logins ADD new INT;
```

### RENAME

```mysql
MariaDB [users]> ALTER TABLE logins RENAME COLUMN new TO old;
```

### MODIFY

```mysql
MariaDB [users]> ALTER TABLE logins MODIFY old DATE;
```

## DROP Statement

```mysql
MariaDB [users]> DROP TABLE logins;
```
