---
title: Msfvenom and Payloads
dg-publish: true
---

# Msfvenom and Payloads

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#Msfvenom|Msfvenom]]
- [[#Encoding Payloads with Msfvenom|Encoding Payloads with Msfvenom]]
- [[#Shellcode|Shellcode]]

---

## Msfvenom

Msfvenom is a command line utility used to generate and encode MSF payloads for various operating systems and web servers. It's a combination of `msfpayload` and `msfencode`. We can generate a malicious meterpreter payload that connects back to our payload handler once executed on the target.

```
msfvenom --list  # list all payloads
```

## Encoding Payloads with Msfvenom

Since the payload is transferred to and stored on the client's disk, attackers need to be conscious of AV detection. Most end-user AV solutions use signature-based detection. We can evade older signature-based AV by encoding our payloads — modifying the payload shellcode to change its signature.

## Shellcode

Shellcode is a piece of code typically used as a payload for exploitation, named after the term "command shell" — it provides an attacker with a remote command shell on the target system.

---

## Automating Metasploit with Resource Scripts

Metasploit resource scripts allow you to automate repetitive tasks and commands, similar to batch scripts. You specify a set of msfconsole commands to execute sequentially, then load the script with msfconsole to automate execution. Useful for setting up multi-handlers and loading/executing payloads.

```
# List pre-packaged resource scripts
ls -al /usr/share/metasploit-framework/scripts/resource

# Automate a multi/handler (example)
vim handler.rc
```
```
use multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.10.5
set LPORT 1234
run
```
```
msfconsole -r handler.rc
```

---

## Windows Exploitation (with MSF)

## External Sources
- [Metasploit Documentation - Modules](https://docs.metasploit.com/docs/modules.html)


## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]
