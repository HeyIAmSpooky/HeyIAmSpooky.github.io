# Penetration Testing Interview Questions - Ultimate Deep Dive

> Based on the Pentester Playbook - 2026 Ultimate Edition

---

## 📚 Table of Contents

1. [Reconnaissance & OSINT](#reconnaissance--osint)
2. [Initial Access](#initial-access)
3. [Exploitation](#exploitation)
4. [Privilege Escalation](#privilege-escalation)
5. [Credential Access & Password Attacks](#credential-access--password-attacks)
6. [Lateral Movement](#lateral-movement)
7. [Active Directory Attacks](#active-directory-attacks)
8. [Persistence](#persistence)
9. [Defense Evasion](#defense-evasion)
10. [Web Application Security](#web-application-security)
11. [Cloud Security](#cloud-security)
12. [Network Attacks](#network-attacks)
13. [Wireless Security](#wireless-security)
14. [Mobile Security](#mobile-security)
15. [Container & Kubernetes Security](#container--kubernetes-security)
16. [ICS/SCADA Security](#icsscada-security)
17. [IoT & Embedded Security](#iot--embedded-security)
18. [Social Engineering](#social-engineering)
19. [Physical Security](#physical-security)
20. [Advanced Attacks](#advanced-attacks)
21. [Tools & Frameworks](#tools--frameworks)
22. [Reporting & Documentation](#reporting--documentation)
23. [Legal & Ethics](#legal--ethics)
24. [Methodology & Process](#methodology--process)

---

## 🔍 Reconnaissance & OSINT

### Subdomain Enumeration

- What is the difference between passive and active subdomain enumeration?
- Explain how certificate transparency logs can be used for subdomain discovery.
- What is the difference between Amass, Subfinder, and Assetfinder?
- How does DNSgen work for subdomain permutation?
- What is the purpose of MassDNS and when would you use it over dnsx?
- How do you validate discovered subdomains for live hosts?
- What is the difference between chaos data and SecurityTrails API?
- How would you enumerate subdomains for a target with strict rate limiting?
- Explain wildcard DNS records and how they affect enumeration.
- What is subdomain takeover and how do you identify vulnerable subdomains?
- How does Shosubgo differ from other subdomain tools?
- What resolver lists do you use for MassDNS and why?

### DNS Enumeration

- What is a DNS zone transfer (AXFR) and why is it a security concern?
- How do you perform DNS zone walking?
- What DNS record types are most valuable during reconnaissance?
- Explain the difference between DNSrecon and DNSenum.
- What is PureDNS and how does it handle DNS resolution?
- How would you enumerate DNS for IPv6 addresses?
- What is the significance of SPF, DKIM, and DMARC records in reconnaissance?
- How do you identify internal hostnames through DNS?
- What is DNS cache snooping and how is it performed?
- Explain passive DNS and its value in reconnaissance.

### Port Scanning & Service Detection

- What is the difference between TCP SYN scan and TCP connect scan?
- Explain how Nmap determines service versions.
- What is the difference between Masscan and Nmap for large-scale scanning?
- How does RustScan integrate with Nmap?
- What are the advantages of Naabu over traditional port scanners?
- How do you scan for UDP services effectively?
- What techniques can you use for firewall/IDS evasion during scanning?
- Explain fragmented packet scanning and decoy scanning.
- How do you identify operating systems through port scanning?
- What is the difference between -sV and -A flags in Nmap?
- How would you scan a /16 network efficiently?
- What are Nmap scripting engine (NSE) categories and when do you use each?
- How do you handle scan rate limiting in enterprise environments?
- What is the significance of TTL values in OS fingerprinting?

### Web Reconnaissance

- What is the difference between httpx and httprobe?
- How does Aquatone perform visual reconnaissance?
- Explain the difference between WhatWeb and Wappalyzer.
- What is WAF detection and which tools do you use?
- How does Nuclei work and what are templates?
- What is the purpose of Katana vs GoSpider vs Hakrawler?
- How do you discover hidden parameters using Arjun or ParamSpider?
- What is JavaScript analysis and how do you extract endpoints from JS files?
- How does LinkFinder discover API endpoints?
- What is the difference between ffuf and Gobuster for content discovery?
- How do you handle rate limiting during directory brute-forcing?
- What wordlists do you use for different types of targets?
- How does Feroxbuster's recursive scanning work?
- What is the purpose of x8 for hidden parameter discovery?

### OSINT & Intelligence Gathering

- How do you use theHarvester for comprehensive email enumeration?
- What is Recon-ng and how does its modular structure work?
- How do you leverage Shodan for reconnaissance?
- What is the difference between Shodan, Censys, and BinaryEdge?
- How do you search for leaked credentials in breach databases?
- What is the purpose of SpiderFoot?
- How do you extract metadata from documents using Metagoofil?
- What Google dorks are most effective for initial reconnaissance?
- How do you enumerate GitHub for sensitive information?
- What is TruffleHog and how does it find secrets in git repos?
- How do you enumerate cloud assets (S3, Azure Blobs, GCP buckets)?
- What social media OSINT techniques are most effective?
- How does CrossLinked work for LinkedIn enumeration?
- What is the purpose of h8mail for email OSINT?

### Network Reconnaissance

- How do you perform ARP scanning on a local network?
- What is NBT scanning and why is it useful in Windows environments?
- How do you enumerate SNMP for valuable information?
- What is IPv6 discovery and why is it often overlooked?
- How do you use Responder for network reconnaissance?
- What is the difference between Bettercap and Ettercap?
- How do you capture and analyze network traffic during reconnaissance?
- What wireless reconnaissance techniques are most effective?
- How do you identify VPN concentrators and their configurations?

---

## 🚪 Initial Access

### Phishing & Social Engineering

- What is the difference between spear phishing and whaling?
- How do you craft effective phishing emails?
- What payload types work best for phishing campaigns?
- How do you bypass email filters and spam detection?
- What is pretexting and how do you develop pretexts?
- How do you set up a convincing phishing infrastructure?
- What is vishing and how do you conduct voice phishing?
- How do you perform SMS phishing (smishing)?
- What tools do you use for phishing (GoPhish, King Phisher)?
- How do you track phishing campaign success metrics?

### Password Attacks

- What is password spraying and how does it differ from brute forcing?
- How do you perform password spraying against Office 365?
- What is the difference between online and offline password attacks?
- How do you avoid account lockout during password attacks?
- What are default credentials and how do you test for them?
- How do you enumerate valid usernames before password attacks?
- What is credential stuffing?
- How do you attack OWA (Outlook Web App)?
- What tools are best for password spraying (Spray, Ruler)?

### Exploitation Vectors

- How do you identify vulnerable services for initial access?
- What is the process for finding and using public exploits?
- How do you modify exploits for specific targets?
- What is the difference between remote and local exploits?
- How do you generate payloads using msfvenom?
- What payload types are available (staged vs stageless)?
- How do you handle exploit reliability issues?
- What is exploit chaining?

---

## 🎯 Exploitation

### Payload Generation

- What is the difference between staged and stageless payloads?
- How do you choose between different payload architectures (x86 vs x64)?
- What shell types are available (bind, reverse, meterpreter)?
- How do you encode payloads to avoid detection?
- What is the purpose of Shikata Ga Nai encoding?
- How do you generate PowerShell payloads?
- What is the difference between web shells and reverse shells?
- How do you create custom payloads to evade AV?
- What is Cobalt Strike and how do its beacons work?
- How do you handle payload execution restrictions?

### Shell Types & Upgrades

- What is the difference between a shell and a terminal?
- How do you upgrade a basic shell to a fully interactive TTY?
- What is pty spawning and why is it important?
- How do you stabilize a reverse shell?
- What is the purpose of rlwrap?
- How do you transfer files through a shell?
- What techniques exist for shell escapes?
- How do you handle shells that die frequently?

### Post-Exploitation Basics

- What are the first commands you run after gaining access?
- How do you identify the current user context?
- What information do you gather for situational awareness?
- How do you check for AV/EDR presence?
- What is the purpose of process enumeration?
- How do you identify network configuration and connectivity?

---

## ⚡ Privilege Escalation

### Windows Privilege Escalation

- What automated tools do you use for Windows privesc enumeration?
- Explain unquoted service paths and how to exploit them.
- What are weak service permissions and how do you identify them?
- How does AlwaysInstallElevated work as a privesc vector?
- What is DLL hijacking and how do you find vulnerable applications?
- How do you exploit stored credentials?
- What is SeImpersonatePrivilege and how do Potato attacks work?
- Explain the difference between JuicyPotato, RoguePotato, PrintSpoofer, and GodPotato.
- What kernel exploits are commonly used for Windows privesc?
- How do you identify missing patches using Watson or WES-NG?
- What is WinPEAS and what does it check for?
- How do you exploit scheduled tasks for privilege escalation?
- What is the purpose of PowerUp and PrivescCheck?
- How do you exploit service binary path manipulation?
- What registry locations contain autorun entries?
- How do you abuse token privileges for escalation?
- What is SeDebugPrivilege and how can it be abused?
- How do you exploit GPO misconfigurations?
- What is LAPS and how can it be abused?
- How do you exploit weak file/folder permissions?

### Linux Privilege Escalation

- What is LinPEAS and what categories does it check?
- How do you identify and exploit SUID binaries?
- What is the significance of capabilities in Linux privesc?
- How do you exploit misconfigured sudo permissions?
- What is GTFOBins and how do you use it?
- How do you exploit writable /etc/passwd or /etc/shadow?
- What are cron job privesc techniques?
- How do you exploit NFS no_root_squash?
- What kernel exploits are commonly used (DirtyCow, DirtyPipe)?
- How do you exploit LD_PRELOAD?
- What is PATH variable hijacking?
- How do you find and exploit writable scripts in cron?
- What is wildcard injection?
- How do you exploit docker group membership?
- What is lxd/lxc privilege escalation?
- How do you identify and exploit weak file permissions?

---

## 🔐 Credential Access & Password Attacks

### Windows Credential Dumping

- How does LSASS memory dumping work?
- What tools can dump LSASS (Mimikatz, Procdump, nanodump)?
- How do you dump SAM, SYSTEM, and SECURITY hives?
- What is NTDS.dit and how do you extract it?
- Explain DCSync attack and its requirements.
- What is the difference between local and domain credential dumping?
- How do you dump credentials remotely using secretsdump?
- What is DPAPI and how do you decrypt protected data?
- How do you extract browser credentials?
- What is LaZagne and what credentials can it extract?
- How do you dump cached domain credentials?
- What is the difference between NT hash and LM hash?
- How do you extract credentials from memory dumps offline?
- What is pypykatz and when do you use it?
- How do you bypass Credential Guard?

### Mimikatz Deep Dive

- What privilege is required for sekurlsa commands?
- Explain sekurlsa::logonpasswords output.
- How do you perform Pass-the-Hash with Mimikatz?
- What is a Golden Ticket and how do you create one?
- What is a Silver Ticket and how does it differ from Golden?
- How do you perform DCSync?
- What is Skeleton Key attack?
- How do you export Kerberos tickets?
- What is Pass-the-Ticket?
- How do you use Mimikatz for DPAPI credential extraction?
- What is sekurlsa::pth and how does it work?

### Password Cracking

- What is the difference between Hashcat and John the Ripper?
- How do you identify hash types?
- What are the most effective wordlists for password cracking?
- How do rule-based attacks work in Hashcat?
- What is mask attack and how do you create effective masks?
- What is a combinator attack?
- How do you crack NTLM hashes efficiently?
- What are the Hashcat modes for different hash types?
- How do you crack Kerberoast tickets?
- How do you crack AS-REP roast hashes?
- What is the difference between attacking NTLMv1 and NTLMv2?
- How do you crack Office document passwords?
- What is rainbow table attack and when is it useful?

### Online Password Attacks

- What is the difference between Hydra, Medusa, and Ncrack?
- How do you attack SSH using Hydra?
- How do you perform HTTP form brute-forcing?
- What is Patator and when do you use it?
- How do you attack RDP passwords?
- What is Crowbar and its specialty?
- How do you handle rate limiting in online attacks?

---

## 🔄 Lateral Movement

### Pass-the-Hash

- What is Pass-the-Hash and why does it work?
- What tools support PTH (Mimikatz, Impacket, CME, Evil-WinRM)?
- How do you perform PTH with CrackMapExec?
- What is the difference between PTH and Pass-the-Key?
- How do you use PTH for RDP with Restricted Admin?
- What are the prerequisites for successful PTH?
- How do you defend against PTH attacks?

### Pass-the-Ticket

- What is Pass-the-Ticket and how does it work?
- How do you export and inject Kerberos tickets?
- What is the difference between TGT and TGS tickets?
- How do you use Rubeus for ticket operations?
- What is ticket delegation and how can it be abused?
- How do you convert between .kirbi and .ccache formats?
- What is Overpass-the-Hash?

### Remote Execution Methods

- What is the difference between PSExec, WMIExec, SMBExec, and AtExec?
- How do you use PowerShell Remoting for lateral movement?
- What is DCOM execution and which objects are commonly abused?
- How do you execute commands via WMI?
- What is the difference between interactive and non-interactive shells?
- How do you create and abuse Windows services for lateral movement?
- What is scheduled task execution?
- How do you use Evil-WinRM for lateral movement?

### SMB & Shares

- How do you enumerate and access SMB shares?
- What is SMB relay and how do you perform it?
- How do you use smbclient and smbmap?
- What is the difference between admin shares (C$, ADMIN$)?
- How do you mount shares with credentials?
- What is SMB signing and how does it affect attacks?

### PowerShell Remoting

- How do you establish PSRemoting sessions?
- What is the difference between Invoke-Command and Enter-PSSession?
- How do you solve the double-hop problem with CredSSP?
- What is JEA (Just Enough Administration)?
- How do you copy files to/from remote sessions?
- What is PowerShell over SSH?

### RDP

- How do you enable RDP remotely?
- What is RDP Restricted Admin mode?
- How do you perform RDP session hijacking?
- What is SharpRDP?
- How do you perform PTH via RDP?

---

## 🏰 Active Directory Attacks

### AD Enumeration

- What is PowerView and what are its key functions?
- How do you enumerate domain users, groups, and computers?
- What is BloodHound and how does data collection work?
- What are the most useful BloodHound queries?
- How do you identify privileged accounts?
- What is AdminCount attribute?
- How do you enumerate Group Policy Objects?
- What is LDAP enumeration and what tools do you use?
- How do you identify trust relationships?
- What is SPN enumeration?

### Kerberos Attacks

- What is Kerberoasting and how do you perform it?
- What is AS-REP Roasting?
- How do you identify Kerberoastable and AS-REProastable accounts?
- What is Unconstrained Delegation?
- What is Constrained Delegation and Resource-Based Constrained Delegation?
- How do you abuse delegation for privilege escalation?
- What is the Printer Bug (SpoolSample)?
- What is PetitPotam and how does it work?
- What are S4U2Self and S4U2Proxy?
- How do you create and use Golden Tickets?
- How do you create and use Silver Tickets?
- What is Diamond Ticket?
- What is the difference between ticket encryption types (RC4 vs AES)?

### Domain Privilege Escalation

- What is DCSync and what permissions are required?
- How do you abuse AdminSDHolder?
- What is GPO abuse and how do you exploit it?
- How do you abuse ACLs for privilege escalation?
- What dangerous rights should you look for (GenericAll, WriteDacl, etc.)?
- How do you exploit group membership for escalation?
- What is LAPS abuse?
- How do you exploit trust relationships?
- What is SID History abuse?
- How do you abuse Azure AD Connect?

### Domain Persistence

- What is the Skeleton Key attack?
- How do you create persistent Golden Tickets?
- What is DCShadow?
- How do you maintain access through AdminSDHolder?
- What are persistence techniques in AD?

---

## 💾 Persistence

### Windows Persistence

- What registry keys are used for autorun persistence?
- How do you create persistent scheduled tasks?
- What is WMI event subscription persistence?
- How do you abuse services for persistence?
- What is DLL hijacking for persistence?
- What are accessibility feature backdoors (Sticky Keys)?
- How do you use COM hijacking?
- What is Bits Jobs persistence?
- How do you use AppInit_DLLs?
- What is Netsh Helper DLL persistence?

### Linux Persistence

- How do you use cron jobs for persistence?
- What is systemd service persistence?
- How do you persist through .bashrc/.profile?
- What is SSH authorized_keys persistence?
- How do you use LD_PRELOAD for persistence?
- What is PAM backdoor?
- How do you create persistent kernel modules?
- What is MOTD script persistence?

---

## 🎭 Defense Evasion

### AV/EDR Evasion

- What techniques are used to evade antivirus?
- How does process injection work?
- What is AMSI and how do you bypass it?
- How do you disable Windows Defender?
- What is ETW and how do you blind it?
- How do you evade EDR hooks?
- What is direct syscalls and why are they useful?
- How do you use unhooking techniques?
- What is process hollowing?
- What is reflective DLL injection?

### Obfuscation

- How do you obfuscate PowerShell scripts?
- What is string encryption?
- How do you modify tool signatures?
- What is living off the land (LOLBins)?
- How do you use legitimate Windows binaries for attacks?

### Log Evasion

- How do you clear Windows event logs?
- What is timestomping?
- How do you disable logging?
- What traces should you clean after an engagement?

---

## 🌐 Web Application Security

### Injection Attacks

- What is SQL injection and what types exist?
- How do you perform blind SQL injection?
- What is time-based SQL injection?
- What is error-based vs union-based SQLi?
- How do you use SQLmap effectively?
- What is NoSQL injection?
- What is LDAP injection?
- What is XML injection and XXE?
- What is command injection?
- How do you exploit template injection (SSTI)?

### XSS (Cross-Site Scripting)

- What is the difference between stored, reflected, and DOM-based XSS?
- How do you bypass XSS filters?
- What are XSS payloads for different contexts?
- How do you escalate XSS to account takeover?
- What is BeEF framework?

### Authentication & Session

- What is authentication bypass?
- How do you attack JWT tokens?
- What is session fixation?
- How do you exploit insecure session management?
- What is CSRF and how do you exploit it?
- How do you bypass 2FA?

### Other Web Attacks

- What is SSRF and how do you exploit it?
- How do you chain SSRF with cloud metadata services?
- What is IDOR (Insecure Direct Object Reference)?
- What is directory traversal/path traversal?
- How do you exploit file upload vulnerabilities?
- What is deserialization and how do you exploit it?
- What is HTTP request smuggling?
- How do you exploit race conditions?
- What is business logic vulnerabilities?

---

## ☁️ Cloud Security

### AWS Security

- How do you enumerate AWS environments?
- What is the AWS metadata service and how do you exploit it?
- What is the difference between IMDSv1 and IMDSv2?
- How do you enumerate S3 buckets?
- What IAM misconfigurations lead to privilege escalation?
- How do you use Pacu for AWS exploitation?
- What is Prowler and ScoutSuite?
- How do you enumerate Lambda functions?
- What are common AWS privilege escalation paths?
- How do you abuse EC2 instance roles?

### Azure Security

- How do you enumerate Azure AD?
- What is the difference between Azure AD and on-prem AD?
- How do you use ROADtools and AADInternals?
- What is MicroBurst?
- How do you enumerate Azure resources?
- What are common Azure privilege escalation techniques?
- How do you abuse Azure automation accounts?
- What is managed identity abuse?

### GCP Security

- How do you enumerate GCP projects?
- What is GCP metadata service exploitation?
- How do you enumerate GCP storage buckets?
- What are GCP IAM privilege escalation techniques?
- How do you abuse service accounts?

---

## 📡 Network Attacks

### Man-in-the-Middle

- What is ARP spoofing?
- How do you use Bettercap for MITM?
- What is DNS spoofing?
- How do you perform LLMNR/NBT-NS poisoning with Responder?
- What is WPAD attack?
- How do you intercept and modify traffic?

### Protocol Attacks

- What is SMB relay?
- How do you perform NTLM relay attacks?
- What is IPv6 attack with mitm6?
- How do you attack SNMP?
- What are common protocol vulnerabilities?

---

## 📶 Wireless Security

### WiFi Attacks

- How do you capture WPA handshakes?
- What is PMKID attack?
- How do you crack WPA/WPA2 passwords?
- What is Evil Twin attack?
- How do you attack WPS?
- What is Karma attack?
- How do you use aircrack-ng suite?
- What is WiFite?

### Bluetooth & Other

- How do you enumerate Bluetooth devices?
- What is BlueBorne?
- What are RFID/NFC attacks?

---

## 📱 Mobile Security

### Android Security

- How do you decompile APK files?
- What is Frida and how do you use it?
- How do you bypass SSL pinning?
- What is root detection bypass?
- How do you analyze mobile traffic?
- What static analysis tools exist (JADX, APKTool)?

### iOS Security

- How do you analyze iOS applications?
- What is Objection?
- How do you bypass jailbreak detection?

---

## 🐳 Container & Kubernetes Security

### Docker Security

- How do you escape Docker containers?
- What is Docker socket abuse?
- What are dangerous container capabilities?
- How do you exploit privileged containers?

### Kubernetes Security

- How do you enumerate Kubernetes clusters?
- What is RBAC abuse?
- How do you exploit misconfigured pods?
- What is container registry attacks?

---

## 🏭 ICS/SCADA Security

- What protocols are used in ICS environments (Modbus, DNP3)?
- How do you enumerate PLC devices?
- What are common ICS vulnerabilities?
- How do you safely test ICS systems?
- What is OPC UA?

---

## 🔌 IoT & Embedded Security

- How do you extract firmware from devices?
- What is UART and JTAG debugging?
- How do you analyze firmware binaries?
- What are common IoT vulnerabilities?
- How do you attack SDR/RF communications?

---

## 🤳 Social Engineering

- What are the phases of a social engineering engagement?
- How do you develop convincing pretexts?
- What is physical social engineering?
- How do you conduct effective vishing campaigns?
- What is USB drop attack?
- How do you bypass physical access controls?

---

## 🔧 Physical Security

- What is lock picking and what tools do you use?
- How do you clone RFID badges?
- What is tailgating/piggybacking?
- How do you perform physical reconnaissance?
- What is a BadUSB attack?

---

## 🚀 Advanced Attacks

### Binary Exploitation

- What is buffer overflow and how do you exploit it?
- What is ROP chain?
- How do you bypass DEP and ASLR?
- What is format string vulnerability?
- What is heap exploitation?

### Supply Chain

- What is dependency confusion?
- How do you attack CI/CD pipelines?
- What is typosquatting?

### Other Advanced Topics

- What is DNS rebinding?
- How do you attack GraphQL APIs?
- What are blockchain/smart contract vulnerabilities?
- What is serverless function exploitation?
- What are side-channel attacks?

---

## 🛠️ Tools & Frameworks

### C2 Frameworks

- What is Cobalt Strike and how does it work?
- What is Sliver?
- What is Havoc?
- How do you choose a C2 framework?
- What is Mythic?

### Enumeration Tools

- How do you use Nmap effectively?
- What is CrackMapExec/NetExec?
- How do you use Impacket suite?
- What is BloodHound/SharpHound?

### Exploitation Frameworks

- How do you use Metasploit?
- What is Cobalt Strike's Aggressor Script?
- How do you develop custom exploits?

---

## 📝 Reporting & Documentation

- What should a professional pentest report include?
- How do you document findings during engagement?
- What is the difference between executive summary and technical details?
- How do you rate vulnerability severity (CVSS)?
- What remediation recommendations should you provide?
- How do you conduct client debriefings?
- What compliance frameworks affect reporting (PCI-DSS, HIPAA)?

---

## ⚖️ Legal & Ethics

- What legal authorizations are required before testing?
- What should a Rules of Engagement document include?
- How do you handle out-of-scope discoveries?
- What is responsible disclosure?
- How do you handle sensitive data found during testing?
- What are the ethics of penetration testing?
- What liability considerations exist?
- How do you handle third-party systems during testing?

---

## 📋 Methodology & Process

### Pre-Engagement

- What information gathering occurs before testing begins?
- How do you define scope and boundaries?
- What communication channels should be established?
- How do you handle emergency contacts?

### During Engagement

- How do you manage time during a pentest?
- When do you escalate findings to the client?
- How do you maintain OPSEC during testing?
- How do you document progress?

### Post-Engagement

- What cleanup activities are required?
- How do you verify all access has been removed?
- What artifacts should be retained or destroyed?
- How do you conduct lessons learned?

---

## 🎓 Scenario-Based Questions

### Real-World Scenarios

- You found a SQL injection but can't get a shell, what do you do?
- You're stuck with a low-privilege shell, walk through your escalation process.
- You compromised a workstation but can't reach the domain controller, what's next?
- You're in an AD environment with no obvious path to DA, what do you check?
- How would you approach a cloud-only environment?
- You find credentials in a file, how do you validate and use them?
- The client reports detection, how do you adjust your approach?
- You found a zero-day during testing, what's your process?
- How do you test a production environment safely?
- You have 24 hours for an internal pentest, how do you prioritize?

---

## 💡 Conceptual & Theory Questions

- Explain the MITRE ATT&CK framework.
- What is the Cyber Kill Chain?
- What is the difference between red team and pentest?
- Explain defense in depth.
- What is the principle of least privilege?
- How does Kerberos authentication work?
- Explain NTLM authentication flow.
- What is PKI and how does certificate trust work?
- What is the difference between symmetric and asymmetric encryption?
- How does TLS/SSL work?

---

_Generated from the Pentester Playbook - 2026 Ultimate Edition_
_Contains 500+ interview questions covering all aspects of penetration testing_
