---
title: DNS Enumeration
dg-publish: true
---

# DNS Enumeration

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#Whois Enumeration|Whois Enumeration]]
- [[#DNS Recon|DNS Recon]]
- [[#Subdomain Enumeration with Sublist3r|Subdomain Enumeration with Sublist3r]]
- [[#DNS Zone Transfers|DNS Zone Transfers]]
- [[#DNS Records|DNS Records]]

---

## Whois Enumeration
WHOIS is a public database that houses information collected when someone registers a domain name. It is used to identify the nameservers of a particular domain.

## DNS Recon
Identify the records related to the domain of the target (dnsdumpster.com)

## Subdomain Enumeration with Sublist3r

## DNS Zone Transfers
DNS zone transfer (query type AXFR) is a type of DNS transaction and one of the mechanisms available for administrators to replicate DNS databases across a set of DNS servers. A zone transfer uses TCP for transport, in the form of a client-server transaction.

## DNS Records
| Record | Purpose |
|---|---|
| A | Resolves a hostname/domain to an IPv4 address |
| AAAA | Resolves a hostname/domain to an IPv6 address |
| NS | Reference to the domain nameserver |
| MX | Resolves a domain to a mail server |
| CNAME | Used for domain aliases |
| TXT | Text record |
| HINFO | Host information |
| SOA | Domain authority |
| SRV | Service records |
| PTR | Resolves an IP address to a hostname |


## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]
