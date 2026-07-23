# SQL Operators

## AND Operator

```mysql
MariaDB [(none)]> SELECT 1 = 1 AND 'test' = 'test';
```

```output title="Output"
+---------------------------+
| 1 = 1 AND 'test' = 'test' |
+---------------------------+
|                         1 |
+---------------------------+
1 row in set (0.001 sec)
```

## OR Operator

```mysql
MariaDB [(none)]> SELECT 1 = 1 OR 'test' = 'abc';
```

```output title="Output"
+-------------------------+
| 1 = 1 OR 'test' = 'abc' |
+-------------------------+
|                       1 |
+-------------------------+
1 row in set (0.001 sec)
```

## NOT Operator

```mysql
MariaDB [(none)]> SELECT NOT 1 = 2;
```

```output title="Output"
+-----------+
| NOT 1 = 2 |
+-----------+
|         1 |
+-----------+
1 row in set (0.001 sec)
```
