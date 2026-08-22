# UNION Clause

## Introduction

UNION ifadesi, birden fazla SELECT ifadesinden elde edilen sonuçları birleştirmek için kullanılır.

SELECT ile ports tablosunu getirelim:

```mysql
MariaDB [ilfreight]> SELECT * FROM ports;
```

```output title="Output"
+--------+-----------+
| code   | city      |
+--------+-----------+
| CN SHA | Shanghai  |
| SG SIN | Singapore |
| ZZ-21  | Shenzhen  |
+--------+-----------+
3 rows in set (0.001 sec)
```

SELECT ile ships tablosunu getirelim:

```mysql
MariaDB [ilfreight]> SELECT * FROM ships;
```

```output title="Output"
+----------+----------+
| ship     | city     |
+----------+----------+
| Morrison | New York |
+----------+----------+
1 row in set (0.001 sec)
```

UNION ile tabloları birleştirelim:

```mysql
MariaDB [ilfreight]> SELECT * FROM ports UNION SELECT * FROM ships;
```

```output title="Output"
+----------+-----------+
| code     | city      |
+----------+-----------+
| CN SHA   | Shanghai  |
| SG SIN   | Singapore |
| ZZ-21    | Shenzhen  |
| Morrison | New York  |
+----------+-----------+
4 rows in set (0.001 sec)
```

## Usage

### Even Columns

UNION ile kullanılan tabloların sütun sayıları eşit olmalıdır. Aksi halde sorgu başarısız olur:

```mysql
MariaDB [ilfreight]> SELECT city FROM ports UNION SELECT * FROM ships;
```

```output title="Output"
ERROR 1222 (21000): The used SELECT statements have a different number of columns
```

### Uneven Columns

Sütun sayıları eşit olmayan tabloları birleştirmek için junk (çöp) veri kullanalım:

!!! warning

    Kullanılan her bir çöp verinin tipi, birleştirilecek olan tablonun her bir sütununda bulunan verinin tipi ile uyumlu olmalıdır

    NULL da kullanılabilir.

```mysql
MariaDB [employees]> SELECT * FROM employees WHERE emp_no = '10001' UNION SELECT dept_no,'2','3','4','5','6' FROM departments;
```

```output title="Output"
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
| 10001  | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
| d001   | 2          | 3          | 4         | 5      | 6          |
| d002   | 2          | 3          | 4         | 5      | 6          |
| d003   | 2          | 3          | 4         | 5      | 6          |
| d004   | 2          | 3          | 4         | 5      | 6          |
| d005   | 2          | 3          | 4         | 5      | 6          |
| d006   | 2          | 3          | 4         | 5      | 6          |
| d007   | 2          | 3          | 4         | 5      | 6          |
| d008   | 2          | 3          | 4         | 5      | 6          |
| d009   | 2          | 3          | 4         | 5      | 6          |
+--------+------------+------------+-----------+--------+------------+
10 rows in set (0.064 sec)
```
