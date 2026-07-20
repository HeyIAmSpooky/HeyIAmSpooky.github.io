---
dg-publish: true
---
**Challenge URL: [TryHackMe | Checkmate](https://tryhackme.com/room/checkmate)

**System Used: THM Attack Box

**Room Task: Marco Bianchi, a systems administrator, recently deployed several internal services, including a firewall console, employee portal, social platform, and SSH access to critical infrastructure. Due to tight deadlines and operational pressure, Marco reused weak, predictable, and pattern-based passwords across multiple systems.  
Your objective is to conduct a password security assessment to identify weaknesses in Marco’s authentication practices.

Start by accessing the main application at http://(IP ADDRESS):5000 From there, you will be guided through each stage of the challenge, uncovering and exploiting weaknesses in Marco’s password usage.

**Written By: Spooky

**Solution:**

1) Level 1 - Marco deployed a firewall at firewall.thm:5001 but kept default credentials.
	1) Browse to the IP Address and Port
	2) ![[Pasted image 20260718113158.png]]
	3) Guess common five letter or number passwords until you figure out what it is.
2) Level 2 - Jobs.thm:5002 -Marco built an internal Employee Login panel on jobs.thm:5002 and used common company keywords as passwords.
	1) Company is MHT Labs
	2) Engineering
	3) ![[Pasted image 20260718120314.png]]
	4) You can guess the password using specific words from the page itself 
3) Level 3 - social.thm:5003 - Navigate to social.thm:5003 and derive Marco's password from personal info - Hint: Use the details from jobs.thm to generate Marco’s password.
	1) ![[Pasted image 20260718120749.png]]
	2) Use CUPP to create a password list that we can use Hydra with the bruteforce
		1) `python3 cupp.py -i`
		2) Enter the information from above when asked and let it run
	3) Use Hydra with the list to brute force the login
		1) `hydra -l Marco.txt -P  social.thm -s 5003 http-post-form "/login:username=^USER^&password=^PASS^:Invalid Credentials"
		2) `host: social.thm   login: marco   password: Bianchi2495`
		3) Remember to add this to the Hosts file if you are having issues with Hydra running properly
4) Level 4 - On social.thm:5003, Marco recently uploaded a new profile picture. For privacy and storage consistency, the platform automatically renames uploaded files to the SHA256 hash of the original filename and saves them in the format (SHA256).png. Your task is to identify the original filename of Marco’s uploaded profile picture. Submit only the filename to proceed.
	1) Once logged in use the dev tools Network tab to locate and find the image
		1) Image Sha-256: d34a569ab7aaa54dacd715ae64953455d86b768846cd0085ef4e9e7471489b7b
	2) Use md5hashing website to decrypt the hash value
5) Level 5 - Marco has revealed his password pattern on social.thm:5003, using predictable rules based on keywords and formatting. Use this information to generate a targeted wordlist and brute-force the SSH service with username marco.
	1) The Pattern : I take a **company keyword**, **capitalize** it, then append the **year** like 2024 or any other number and an **exclamation mark**.
	2) ``` Custom-Word-List
	   for k in PUT COMMON COMPANY KEYWORDS HERE; do  
		  for y in PUT YEARS HERE; do  
		    echo "$(tr '[:lower:]' '[:upper:]' <<< ${k:0:1})${k:1}$y!"  
		  done
		done > ssh_words.txt
		```
	3) Use Hydra to brute force the ssh login
		1) `hydra -l marco -P ssh_words.txt ssh://IPADDRESS`