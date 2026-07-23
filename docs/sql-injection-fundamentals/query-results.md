# Query Results

## ORDER BY Clause

Sonuçlar sütun ismi veya sütun numarası kullanılarak sıralanabilir:

=== "NAME"

    ```mysql
    MariaDB [employees]> SELECT * FROM departments ORDER BY dept_name;
    ```

=== "NUMBER"

    ```mysql
    MariaDB [employees]> SELECT * FROM departments ORDER BY 2;
    ```

```output title="Output"
+---------+--------------------+
| dept_no | dept_name          |
+---------+--------------------+
| d009    | Customer Service   |
| d005    | Development        |
| d002    | Finance            |
| d003    | Human Resources    |
| d001    | Marketing          |
| d004    | Production         |
| d006    | Quality Management |
| d008    | Research           |
| d007    | Sales              |
+---------+--------------------+
9 rows in set (0.073 sec)
```

Sonuçlar artan veya azalan sırada sıralanabilir:

=== "ASCENDING"

    ```mysql
    MariaDB [users]> SELECT * FROM logins ORDER BY password ASC;
    ```

    ```output title="Output"
    +----+---------------+------------+---------------------+
    | id | username      | password   | date_of_joining     |
    +----+---------------+------------+---------------------+
    |  2 | administrator | adm1n_p@ss | 2024-11-10 10:24:51 |
    |  3 | john          | john123!   | 2024-11-10 10:24:55 |
    |  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
    |  4 | tom           | tom123!    | 2024-11-10 10:24:55 |
    +----+---------------+------------+---------------------+
    4 rows in set (0.001 sec)
    ```

=== "DESCENDING"

    ```mysql
    MariaDB [users]> SELECT * FROM logins ORDER BY password DESC;
    ```

    ```output title="Output"
    +----+---------------+------------+---------------------+
    | id | username      | password   | date_of_joining     |
    +----+---------------+------------+---------------------+
    |  4 | tom           | tom123!    | 2024-11-10 10:24:55 |
    |  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
    |  3 | john          | john123!   | 2024-11-10 10:24:55 |
    |  2 | administrator | adm1n_p@ss | 2024-11-10 10:24:51 |
    +----+---------------+------------+---------------------+
    4 rows in set (0.001 sec)
    ```

## LIMIT Clause

```mysql
MariaDB [users]> SELECT * FROM logins LIMIT 2;
```

```output title="Output"
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  2 | administrator | adm1n_p@ss | 2024-11-10 10:24:51 |
+----+---------------+------------+---------------------+
2 rows in set (0.001 sec)
```

## WHERE Clause

```mysql
MariaDB [users]> SELECT * FROM logins WHERE username = 'admin';
```

```output title="Output"
+----+----------+----------+---------------------+
| id | username | password | date_of_joining     |
+----+----------+----------+---------------------+
|  1 | admin    | p@ssw0rd | 2020-07-02 00:00:00 |
+----+----------+----------+---------------------+
1 row in set (0.001 sec)
```

## LIKE Clause

Sıfır ya da daha fazla karakter ile eşleşen sonuçlar için % wildcard kullanılır:

```mysql
MariaDB [users]> SELECT * FROM logins WHERE username LIKE 'admin%';
```

```output title="Output"
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  2 | administrator | adm1n_p@ss | 2024-11-10 10:24:51 |
+----+---------------+------------+---------------------+
2 rows in set (0.001 sec)
```

Tam olarak bir karakter ile eşleşen sonuçlar için _ kullanılır:

```mysql
MariaDB [users]> SELECT * FROM logins WHERE username LIKE '___';
```

```output title="Output"
+----+----------+----------+---------------------+
| id | username | password | date_of_joining     |
+----+----------+----------+---------------------+
|  4 | tom      | tom123!  | 2024-11-10 10:24:55 |
+----+----------+----------+---------------------+
1 row in set (0.001 sec)
```
