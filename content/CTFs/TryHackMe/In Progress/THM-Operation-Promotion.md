---
dg-publish: true
---
Room Description: You are up for promotion at **Hadron Security**. Your senior lead, Mara, has handed you a solo engagement against **RecruitCorp**, a small recruiting firm with a public-facing portal. Compromise the host, capture the flags, and demonstrate that you are ready for the Penetration Tester title.

System Info: Parrot OS 7.2 VMWare Workstation Player


1) NMAP Scan
		1) ![[Pasted image 20260629110439.png]]
2) GOBUSTER Scan 
		1) ![[Pasted image 20260629111725.png]]
		2) ![[Pasted image 20260723105140.png]]
			1) Robots disallows /admin/
			2) 
3) Browse to admin portal found in GoBuster Scan
	1) ![[Pasted image 20260629113403.png]]
		1) I attempted to login using basic credentials with no luck
		2) I then attempted SQL Injection
			1) `admin' OR '1'='1' --`
				1) ![[Pasted image 20260629113641.png]]
4) Attempted user lookup and noticed potential PHP exploit for root access
	1) ![[Pasted image 20260629114427.png]]
	2) ![[Pasted image 20260629115055.png]]
5) Reverse Shell?

