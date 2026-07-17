---
title: Website Footprinting
dg-publish: true
---

# Website Footprinting

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#Website Recon & Footprinting|Website Recon & Footprinting]]
- [[#Website Footprinting with Netcraft|Website Footprinting with Netcraft]]
- [[#WAF Detection with wafw00f|WAF Detection with wafw00f]]
- [[#Google Dorks|Google Dorks]]
- [[#Harvester|Harvester]]
- [[#Have I Been Pwned|Have I Been Pwned]]

---

## Website Recon & Footprinting

What to look for:
- IP address
- Directories hidden from search engines
- Names
- E-mail
- Phone number
- Physical address
- Web technology

**To find IP address:** `$ host URL`

**robots.txt** — allows you to specify what folders or files you don't want search engines to index (`URL/robots.txt`)

**sitemap.xml** — provides search engines an organized way to index (`URL/sitemap.xml`)

**whatweb** — used to do a stealthy scan, which gives the web technologies including content management system (CMS), packages, JavaScript libraries, web servers, and embedded devices. It also gives version numbers, e-mail addresses, account IDs, etc.

**HTTrack** — used to download an entire website (Install: `sudo apt-get install httrack`)

## Website Footprinting with Netcraft
Used to do a lookup on the target; gives information about the website.

## WAF Detection with wafw00f
WAF is a Web Application Firewall. It can be detected with a tool called **WAFW00F** (web application firewall fingerprinting tool).

## Google Dorks
- `site:url`
- `inurl:admin` (sample)
- `site:*` (subdomain)
- `filetype:doc`
- `intitle:`

## Harvester
Used to harvest email: `theHarvester -d url -b search_engine`

## Have I Been Pwned
Check for leaked passwords.

---

## Active Information Gathering

## External Sources
- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)


## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]
