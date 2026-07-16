---
dg-publish: true
---
**Challenge URL: [Office Password Cracker | flags | HackerDNA](https://hackerdna.com/labs/office-password-cracker)**

**System Used: Windows Subsystem for Linux - Parrot OS**

**Room Task: A password-protected Word document stands between you and critical information. The file is encrypted, the contents hidden behind a corporate password. Armed with the right tools and techniques, can you break through the protection and uncover what lies within? Time to put your password cracking skills to the test.**


**Solution**
1) Launch the lab, browse to the site, and download the password protected document that is given. 
2) Its protected so opening does nothing for us. We need to extract the hash from the document to continue
3) Use Office2John (Part of John The Ripper) to extract the hash from the document
	1) `/usr/share/john/office2john.py confidential_report.docx > office_hash.txt`
	2) Make sure to note where this document is created as its needed in the next step. I saved mine as office_hash.txt to separate it from other files
4) Open up the new text file with the extracted hash. Clean up the parts that are not a component of the hash and save the file again with the changes
5) Use hashcat to extract the password of the hash file
	1) The mode for the specific hash we have is 9600
	2) `hashcat -m 9600 -a 0 office_hash.txt /usr/share/wordlists/rockyou.txt`
6) Wait for hashcat to find the password and note it down.
7) Open the office document and enter the password from step 6.
8) Congrats you found the flag!