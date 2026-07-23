# Databases

## Setting Up the Database

### Start PostgreSQL

```sh
my@attack:~$ sudo systemctl start postgresql
```

### Initiate a Database

```sh
my@attack:~$ msfdb init
```

### Database Options

```sh
msf6 > help database
```

```output title="Output"
Database Backend Commands
=========================

    Command           Description
    -------           -----------
    analyze           Analyze database information about a specific address or address range
    db_connect        Connect to an existing data service
    db_disconnect     Disconnect from the current data service
    db_export         Export a file containing the contents of the database
    db_import         Import a scan result file (filetype will be auto-detected)
    db_nmap           Executes nmap and records the output automatically
    db_rebuild_cache  Rebuilds the database-stored module cache (deprecated)
    db_remove         Remove the saved data service entry
    db_save           Save the current data service connection as the default to reconnect on startup
    db_stats          Show statistics for the database
    db_status         Show the current data service status
    hosts             List all hosts in the database
    klist             List Kerberos tickets in the database
    loot              List all loot in the database
    notes             List all notes in the database
    services          List all services in the database
    vulns             List all vulnerabilities in the database
    workspace         Switch between database workspaces
```

## Workspaces

### Show Workspaces

```sh
msf6 > workspace
```

### Create a Workspace

```sh
msf6 > workspace -a Target_1
```

### Choose a Workspace

```sh
msf6 > workspace Target_1
```

## Importing Scan Results

```sh
msf6 > db_import results.xml
```

## Nmap

```sh
msf6 > db_nmap 10.10.10.40 -sS -sV
```

## Data Backup

```sh
msf6 > db_export -f xml backup.xml
```

## Stored Hosts

```sh
msf6 > hosts
```

```output title="Output"
Hosts
=====

address    mac  name       os_name  os_flavor  os_sp  purpose  info  comments
-------    ---  ----       -------  ---------  -----  -------  ----  --------
127.0.0.1       localhost  Unknown                    device
```

## Stored Services of Hosts

```sh
msf6 > services
```

```output title="Output"
Services
========

host       port  proto  name        state  info
----       ----  -----  ----        -----  ----
127.0.0.1  22    tcp    ssh         open   OpenSSH 9.9 protocol 2.0
127.0.0.1  5432  tcp    postgresql  open   PostgreSQL DB 16.0 - 16.2
```

## Stored Credentials

```sh
msf6 > creds -h
```

```output title="Output"
With no sub-command, list credentials. If an address range is
given, show only credentials with logins on hosts within that
range.

Usage - Listing credentials:
  creds [filter options] [address range]

Usage - Adding credentials:
  creds add uses the following named parameters.
    user      :  Public, usually a username
    password  :  Private, private_type Password.
    ntlm      :  Private, private_type NTLM Hash.
    postgres  :  Private, private_type postgres MD5
    pkcs12    :  Private, private_type pkcs12 archive file, must be a file path.
    ssh-key   :  Private, private_type SSH key, must be a file path.
    hash      :  Private, private_type Nonreplayable hash
    jtr       :  Private, private_type John the Ripper hash type.
    realm     :  Realm,
    realm-type:  Realm, realm_type (domain db2db sid pgdb rsync wildcard), defaults to domain.

Examples: Adding
   # Add a user, password and realm
   creds add user:admin password:notpassword realm:workgroup
   # Add a user and password
   creds add user:guest password:'guest password'
   # Add a password
   creds add password:'password without username'
   # Add a user with an NTLMHash
   creds add user:admin ntlm:E2FC15074BF7751DD408E6B105741864:A1074A69B1BDE45403AB680504BBDD1A
   # Add a NTLMHash
   creds add ntlm:E2FC15074BF7751DD408E6B105741864:A1074A69B1BDE45403AB680504BBDD1A
   # Add a Postgres MD5
   creds add user:postgres postgres:md5be86a79bf2043622d58d5453c47d4860
   # Add a user with a PKCS12 file archive
   creds add user:alice pkcs12:/path/to/certificate.pfx
   # Add a user with an SSH key
   creds add user:sshadmin ssh-key:/path/to/id_rsa
   # Add a user and a NonReplayableHash
   creds add user:other hash:d19c32489b870735b5f587d76b934283 jtr:md5
   # Add a NonReplayableHash
   creds add hash:d19c32489b870735b5f587d76b934283

General options
  -h,--help             Show this help information
  -o <file>             Send output to a file in csv/jtr (john the ripper) format.
                        If file name ends in '.jtr', that format will be used.
                        If file name ends in '.hcat', the hashcat format will be used.
                        csv by default.
  -d,--delete           Delete one or more credentials

Filter options for listing
  -P,--password <text>  List passwords that match this text
  -p,--port <portspec>  List creds with logins on services matching this port spec
  -s <svc names>        List creds matching comma-separated service names
  -u,--user <text>      List users that match this text
  -t,--type <type>      List creds of the specified type: password, ntlm, hash or any valid JtR format
  -O,--origins <IP>     List creds that match these origins
  -r,--realm <realm>    List creds that match this realm
  -R,--rhosts           Set RHOSTS from the results of the search
  -v,--verbose          Don't truncate long password hashes

Examples, John the Ripper hash types:
  Operating Systems (starts with)
    Blowfish ($2a$)   : bf
    BSDi     (_)      : bsdi
    DES               : des,crypt
    MD5      ($1$)    : md5
    SHA256   ($5$)    : sha256,crypt
    SHA512   ($6$)    : sha512,crypt
  Databases
    MSSQL             : mssql
    MSSQL 2005        : mssql05
    MSSQL 2012/2014   : mssql12
    MySQL < 4.1       : mysql
    MySQL >= 4.1      : mysql-sha1
    Oracle            : des,oracle
    Oracle 11         : raw-sha1,oracle11
    Oracle 11 (H type): dynamic_1506
    Oracle 12c        : oracle12c
    Postgres          : postgres,raw-md5

Examples, listing:
  creds               # Default, returns all credentials
  creds 1.2.3.4/24    # Return credentials with logins in this range
  creds -O 1.2.3.4/24 # Return credentials with origins in this range
  creds -p 22-25,445  # nmap port specification
  creds -s ssh,smb    # All creds associated with a login on SSH or SMB services
  creds -t ntlm       # All NTLM creds

Example, deleting:
  # Delete all SMB credentials
  creds -d -s smb
```

## Stored Loot

```sh
msf6 > loot -h
```

```output title="Output"
Usage: loot [options]
 Info: loot [-h] [addr1 addr2 ...] [-t <type1,type2>]
  Add: loot -f [fname] -i [info] -a [addr1 addr2 ...] -t [type]
  Del: loot -d [addr1 addr2 ...]


OPTIONS:

    -a, --add                 Add loot to the list of addresses, instead of listing.
    -d, --delete              Delete *all* loot matching host and type.
    -f, --file <filename>     File with contents of the loot to add.
    -h, --help                Show this help information.
    -i, --info <info>         Info of the loot to add.
    -S, --search <filter>     Search string to filter by.
    -t, --type <type1,type2>  Search for a list of types.
    -u, --update              Update loot. Not officially supported.
```
