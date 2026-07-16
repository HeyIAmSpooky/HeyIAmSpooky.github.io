---
dg-publish: true
---
# SQL Commands

```bash
MySQL Basics
MySQL version
mysql -h (IPaddress) -u (username)
use auxiliary/scanner/mysql/mysql_schemadump
use auxiliary/scanner/mysql/mysql_writable_dirs
use auxiliary/scanner/mysql/mysql_file_enum
mysql -u (username) -h (IPaddress)
use auxiliary/scanner/mysql/mysql_hashdump
nmap --script mysql-empty-password -p3306 (IPaddress)
nmap --script mysql-info -p3306 (IPaddress)
nmap --script mysql-users --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-databases --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-variables --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-audit --script-args mysql-audit.username='root',mysql-audit.password="",mysql-audit.filename='location' -p3306 (IPaddress)
nmap --script mysql-dump-hashes --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
use auxiliary/scanner/mysql/mysql_login
hydra -l (username) -P (file location) (IPaddress) mysql
MySQL Enumeration
MySQL is an open-source relational database management system based on SQL. Uses TCP port 3306 by default. We can use auxiliary modules to enumerate MySQL version, brute-force passwords, execute SQL queries, and more.
```
