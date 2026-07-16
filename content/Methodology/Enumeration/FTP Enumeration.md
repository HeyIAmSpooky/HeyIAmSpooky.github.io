---
title: FTP Enumeration
dg-publish: true
---

# FTP Enumeration

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#FTP Enumeration|FTP Enumeration]]

---

## FTP Enumeration

FTP uses TCP port 21 and facilitates file sharing between a server and clients. We can use multiple auxiliary modules to enumerate information and perform brute-force attacks. FTP authentication uses a username/password combination, though improperly configured servers can sometimes be accessed anonymously.


## Using Terminal
```
Find version of FTP
nmap -p21 -sV -O (IPAddress)

# Brute force
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.67.22.3 -t 4 ftp

# Find whether anonymous login is allowed
nmap (IPaddress) -p21 --script ftp-anon
```


## Using MSF

```
# Service version of FTP
use auxiliary/scanner/ftp/ftp_version
set RHOSTS (IPaddress)
run

# Bruteforce attack on FTP
use auxiliary/scanner/ftp/ftp_login
set RHOSTS (IPAddress)
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run

# Check if anonymous logons are allowed
use auxiliary/scanner/ftp/anonymous
set RHOSTS (IPAddress)
run
```

