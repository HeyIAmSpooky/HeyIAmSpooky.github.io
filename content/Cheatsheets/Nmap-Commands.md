---
dg-publish: true
---
# Nmap Commands

```bash
sudo nmap -sn ip/subnet
nmap ipaddress                # default scanning
nmap -Pn ipaddress            # resolve the ping problem
nmap -p445 --script smb-protocols (IPaddress)
nmap -p445 --script smb-security-mode (IPaddress)
nmap -p445 --script smb-enum-sessions (IPaddress)
nmap -p445 --script smb-enum-sessions --script-args smbusername=(username),smbpassword=(password) (IPaddress)
nmap -p445 --script smb-enum-shares (IPaddress)
nmap -p445 --script smb-enum-users --script-args smbusername=(username),smbpassword=(password) (IPaddress)
nmap -p445 --script smb-enum-domains --script-args smbusername=(username),smbpassword=(password) (IPaddress)
nmap -sS (IPaddress)
nmap -sU --top-ports 25 (IPaddress)
nmap -sV -p445 (IPaddress)
nmap (IPaddress) -p 445 --script smb-os-discovery
nmap --script smb-enum-users.nse -p445 (IPaddress)
nmap (IPaddress) --script smb-enum-shares -p445
nmap -p21 -sV -O 192.67.22.3
nmap (IPaddress) -p21 --script ftp-anon
nmap -p3306 -sV (IPaddress)
nmap --script mysql-empty-password -p3306 (IPaddress)
nmap --script mysql-info -p3306 (IPaddress)
nmap --script mysql-users --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-databases --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-variables --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-audit --script-args mysql-audit.username='root',mysql-audit.password="",mysql-audit.filename='location' -p3306 (IPaddress)
nmap --script mysql-dump-hashes --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script ms-sql-info -p 1433 (IPaddress)
nmap -p1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 (IPaddress)
nmap -p 1433 --script ms-sql-brute --script-args userdb=(location) (IPaddress)
nmap -p1433 --script ms-sql-empty-password (IPaddress)
nmap -sV -p445 --script=smb-vuln-ms17-010 (IPaddress)
nmap -p 3389 (IPaddress)
Nmap is a free and open-source network scanner used to discover hosts on a network and scan targets for open ports. It can also enumerate services running on open ports and the operating system running on the target. We can output Nmap scan results in a format importable into MSF for vulnerability detection and exploitation.
db_nmap -sS -sV -O (ipaddress)
db_nmap -sS -sV -O (IPaddress)
db_nmap -sV (IPaddress)
nmap -sV -O (IPaddress)
nmap -sV --script=banner (IPaddress)
nmap -sV 10.0.22.85
nmap -T4 -Pn -sC -sV -p 1-10000 10.0.22.85
nmap -sV -sC -p 445 10.0.22.85
nmap 10.0.23.180
Nmap scan report for 10.0.23.180
Nmap done: 1 IP address (1 host up) scanned in 2.68 seconds
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.51 seconds
```
