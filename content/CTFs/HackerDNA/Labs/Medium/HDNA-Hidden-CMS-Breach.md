---
tags:
  - CTF
  - THM
  - Writeup
---
Lab Explanation: A web server sits quietly in the corner of the network, but something tells you there's more than meets the eye. 🌐 The homepage reveals little, yet beneath the surface lies a complete application waiting to be discovered. Through careful enumeration and exploitation, can you turn a simple web server into full system access? Sometimes the best secrets are the ones hiding in plain sight. 🔓

Lab Link: [Hidden CMS Breach | flags | HackerDNA](https://hackerdna.com/labs/hidden-cms-breach)

1) Browsed to site to view webpage
	1) ![[Pasted image 20260624092737.png]]
		1) Nothing was found in Dev Tools (F12)
		2) We now know the location of the two flags
		3) /home/flag_user.txt
		4) /root/flag_root.txt
	2) Run NMAP against the target
		1) Results:
		2) ![[Pasted image 20260624093123.png]]
	3) User CURL on Robots.TXT
		1) ![[Pasted image 20260624094427.png]]
	4) Browsed to /new_website? page and found additional CMS Page
	5) Adjusted URL to include /Admin and I found a login page
	6) GetSimple CMS has a known flaw where you can browse to `/users/data/admin.xml`
	7) ![[Pasted image 20260720102756.png]]
		1) SHA1 hash for PWD
	8) Use John to decrypt the hash
		1) `john --format=raw-sha1 hash.txt --wordlist=~/wordlists/rockyou.txt`
			1) 1234yellow
	9) Login to Admin Portal with credentials
	10) Review the portal to see where we might be able to exploit
		1) Themes in CMS run php commands and we can edit the file
		2) insert PHP one liner and attempt RCE
			1) `curl -s "http://$TARGET/new_website/theme/Innovation/template.php?cmd=id " -H "Host: getsimple.hdna"`
			2) confirmed RCE
		3) Use the RCE to read the user flag and enter onto website
	11) Review permissions for sudo 
		1) `cmd=sudo%20-l`
			1) %20 = Spaces
			2) 