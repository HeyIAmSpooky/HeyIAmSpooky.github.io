---
dg-publish: true
---
Challenge URL: [Fine Print | flags | HackerDNA](https://hackerdna.com/labs/fine-print)

System Used: WSL Parrot OS 7.2

Room Task: CipherCorp's corporate compliance document looks like standard legal boilerplate. But buried in the stylesheet, a CSS media query conceals a confidential internal memo that only appears in a different rendering context. Can you read between the lines and find what was never meant to be seen on screen?

Written By: Spooky

Solution:
1) Launch the lab and access the website
2) Press F12 to view the Developer Tools
3) Browsing the Elements tab we see that there is a section called Print Only with the title Confidential Internal Memo
4) Exit the Dev Tools and Print the document to reveal the confidential memo
5) Read the location of the flag, add it to the URL, and input your flag into the console