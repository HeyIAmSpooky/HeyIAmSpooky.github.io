---
title: Metasploit Enumeration and Scanning
dg-publish: true
---

# Metasploit Enumeration and Scanning

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#Auxiliary Modules|Auxiliary Modules]]
- [[#Vulnerability Scanning|Vulnerability Scanning]]
- [[#Vulnerability Scanning with Nessus|Vulnerability Scanning with Nessus]]
- [[#WMAP|WMAP]]

---

## Auxiliary Modules

Auxiliary modules perform functionality like scanning, discovery, and fuzzing. We can use them for both TCP & UDP port scanning as well as enumerating information from services like FTP, SSH, HTTP, etc. Auxiliary modules can be used during the information gathering phase as well as the post-exploitation phase.

## Vulnerability Scanning

Vulnerability scanning & detection is the process of scanning a target for vulnerabilities and verifying whether they can be exploited.

```
searchsploit "" (used to search for exploits)
```
https://github.com/hahwul/metasploit-autopwn

## Vulnerability Scanning with Nessus

Nessus is a proprietary vulnerability scanner developed by Tenable. We can perform a vulnerability scan on a target system, then import Nessus results into MSF for analysis and exploitation. Nessus automates the process of identifying vulnerabilities and provides info like the CVE code. The free version (Nessus Essentials) allows scanning up to 16 IPs.

## WMAP

WMAP is a powerful, feature-rich web application vulnerability scanner that automates web server enumeration and scans web apps for vulnerabilities. Available as an MSF plugin, fully integrated with MSF.

---

## Client-Side Attacks

A client-side attack involves coercing a client to execute a malicious payload on their system, which connects back to the attacker when executed. Typically utilizes social engineering techniques like malicious documents or portable executables (PEs). These attacks exploit human vulnerabilities rather than service/software vulnerabilities. Attackers need to be conscious of AV detection since the payload is transferred to and stored on the client's disk.

## External Sources
- [Metasploit Documentation - Modules](https://docs.metasploit.com/docs/modules.html)


## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]
