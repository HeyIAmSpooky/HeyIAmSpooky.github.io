---
dg-publish: false
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