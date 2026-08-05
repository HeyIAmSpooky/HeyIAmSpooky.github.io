``` -sV_nmap_scan
Nmap scan report for ip-192-168-100-50.us-west-1.compute.internal (192.168.100.50)
Host is up (0.00026s latency).
Not shown: 990 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Apache httpd 2.4.51 ((Win64) PHP/7.4.26)
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49160/tcp open  msrpc              Microsoft Windows RPC
MAC Address: 06:55:D2:2F:AB:81 (Unknown)
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Nmap scan report for ip-192-168-100-51.us-west-1.compute.internal (192.168.100.51)
Host is up (0.00042s latency).
Not shown: 988 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
21/tcp    open  ftp                Microsoft ftpd
80/tcp    open  http               Microsoft IIS httpd 8.5
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49160/tcp open  msrpc              Microsoft Windows RPC
49161/tcp open  msrpc              Microsoft Windows RPC
MAC Address: 06:49:60:C6:33:4D (Unknown)
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:window

Nmap scan report for ip-192-168-100-52.us-west-1.compute.internal (192.168.100.52)
Host is up (0.00020s latency).
Not shown: 992 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           vsftpd 3.0.3
22/tcp   open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
25/tcp   open  smtp          OpenSMTPD
80/tcp   open  http          Apache httpd 2.4.41
139/tcp  open  netbios-ssn   Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn   Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
3306/tcp open  mysql         MySQL 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
3389/tcp open  ms-wbt-server xrdp
MAC Address: 06:A1:76:A7:82:47 (Unknown)
Service Info: Hosts: 1c5ce6f62bfc, IP-192-168-100-52; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Nmap scan report for ip-192-168-100-55.us-west-1.compute.internal (192.168.100.55)
Host is up (0.00055s latency).
Not shown: 995 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
MAC Address: 06:F2:4B:DD:22:D7 (Unknown)
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Nmap scan report for ip-192-168-100-63.us-west-1.compute.internal (192.168.100.63)
Host is up (0.00026s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
3389/tcp open  ms-wbt-server Microsoft Terminal Services
MAC Address: 06:51:A7:E4:EF:E7 (Unknown)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Nmap scan report for ip-192-168-100-67.us-west-1.compute.internal (192.168.100.67)
Host is up (0.00015s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION52
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
MAC Address: 06:95:41:60:97:0D (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Nmap scan report for ip-192-168-100-5.us-west-1.compute.internal (192.168.100.5)
Host is up (0.0000030s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
22/tcp   open  ssh           OpenSSH 8.7p1 Debian 2 (protocol 2.0)
3389/tcp open  ms-wbt-server xrdp
5910/tcp open  vnc           VNC (protocol 3.8)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

``` -A_nmap_scan_52
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 65534    65534         318 Apr 18  2022 updates.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.100.5
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 32:fd:a2:54:c4:a5:4b:b4:1a:8f:4a:34:58:e1:48:10 (RSA)
|   256 53:42:7c:e2:6f:91:e5:01:2e:83:3b:3d:00:50:34:ff (ECDSA)
|_  256 d3:ac:0c:37:6b:38:56:3e:fd:e7:10:7d:0b:27:8d:04 (ED25519)
25/tcp   open  smtp          OpenSMTPD
| smtp-commands: 1c5ce6f62bfc Hello ip-192-168-100-52.us-west-1.compute.internal [192.168.100.5], pleased to meet you, 8BITMIME, ENHANCEDSTATUSCODES, SIZE 36700160, DSN, HELP
|_ 2.0.0 This is OpenSMTPD 2.0.0 To report bugs in the implementation, please contact bugs@openbsd.org 2.0.0 with full details 2.0.0 End of HELP info
80/tcp   open  http          Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Index of /
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2018-02-21 17:28  drupal/
|_
139/tcp  open  netbios-ssn   Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn   Samba smbd 4.13.17-Ubuntu (workgroup: WORKGROUP)
3306/tcp open  mysql         MySQL 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
|   Thread ID: 38
|   Capabilities flags: 63486
|   Some Capabilities: Support41Auth, IgnoreSigpipes, IgnoreSpaceBeforeParenthesis, DontAllowDatabaseTableColumn, Speaks41ProtocolOld, LongColumnFlag, Speaks41ProtocolNew, ConnectWithDatabase, SupportsTransactions, FoundRows, SupportsLoadDataLocal, SupportsCompression, InteractiveClient, ODBCClient, SupportsMultipleResults, SupportsAuthPlugins, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: Jw<`QT4FD`86D5z*\sPr
|_  Auth Plugin Name: mysql_native_password
3389/tcp open  ms-wbt-server xrdp
MAC Address: 06:A1:76:A7:82:47 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=8/1%OT=21%CT=1%CU=35436%PV=Y%DS=1%DC=D%G=Y%M=06A176%TM
OS:=6A6E1435%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10D%TI=Z%CI=Z%II=I%
OS:TS=A)OPS(O1=M2301ST11NW7%O2=M2301ST11NW7%O3=M2301NNT11NW7%O4=M2301ST11NW
OS:7%O5=M2301ST11NW7%O6=M2301ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4
OS:B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M2301NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=
OS:40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%
OS:O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=4
OS:0%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%
OS:Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=
OS:Y%DFI=N%T=40%CD=S)

Network Distance: 1 hop
Service Info: Hosts: 1c5ce6f62bfc, IP-192-168-100-52; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: IP-192-168-100-, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-time: 
|   date: 2026-08-01T15:43:49
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.13.17-Ubuntu)
|   Computer name: ip-192-168-100-52
|   NetBIOS computer name: IP-192-168-100-52\x00
|   Domain name: us-west-1.compute.internal
|   FQDN: ip-192-168-100-52.us-west-1.compute.internal
|_  System time: 2026-08-01T15:43:49+00:00

TRACEROUTE
HOP RTT     ADDRESS
1   0.76 ms ip-192-168-100-52.us-west-1.compute.internal (192.168.100.52)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.24 second
```

``` ftp-updatestxt-file
root@kali:~# cat updates.txt
Greetings gentlemen!

- I have setup the server successfully and have configured Drupal.
- Your Drupal usernames are exactly the same as your user account passwords on this server. Contact me to get your Drupal passwords.
- I was too busy to setup a file sharing server so i will be posting the updates here.

- admin
```

![[Pasted image 20260801103149.png]]


```
#
# robots.txt
#
# This file is to prevent the crawling and indexing of certain parts
# of your site by web crawlers and spiders run by sites like Yahoo!
# and Google. By telling these "robots" where not to go on your site,
# you save bandwidth and server resources.
#
# This file will be ignored unless it is at the root of your host:
# Used:    http://example.com/robots.txt
# Ignored: http://example.com/site/robots.txt
#
# For more information about the robots.txt standard, see:
# http://www.robotstxt.org/robotstxt.html

User-agent: *
Crawl-delay: 10
# CSS, JS, Images
Allow: /misc/*.css$
Allow: /misc/*.css?
Allow: /misc/*.js$
Allow: /misc/*.js?
Allow: /misc/*.gif
Allow: /misc/*.jpg
Allow: /misc/*.jpeg
Allow: /misc/*.png
Allow: /modules/*.css$
Allow: /modules/*.css?
Allow: /modules/*.js$
Allow: /modules/*.js?
Allow: /modules/*.gif
Allow: /modules/*.jpg
Allow: /modules/*.jpeg
Allow: /modules/*.png
Allow: /profiles/*.css$
Allow: /profiles/*.css?
Allow: /profiles/*.js$
Allow: /profiles/*.js?
Allow: /profiles/*.gif
Allow: /profiles/*.jpg
Allow: /profiles/*.jpeg
Allow: /profiles/*.png
Allow: /themes/*.css$
Allow: /themes/*.css?
Allow: /themes/*.js$
Allow: /themes/*.js?
Allow: /themes/*.gif
Allow: /themes/*.jpg
Allow: /themes/*.jpeg
Allow: /themes/*.png
# Directories
Disallow: /includes/
Disallow: /misc/
Disallow: /modules/
Disallow: /profiles/
Disallow: /scripts/
Disallow: /themes/
# Files
Disallow: /CHANGELOG.txt
Disallow: /cron.php
Disallow: /INSTALL.mysql.txt
Disallow: /INSTALL.pgsql.txt
Disallow: /INSTALL.sqlite.txt
Disallow: /install.php
Disallow: /INSTALL.txt
Disallow: /LICENSE.txt
Disallow: /MAINTAINERS.txt
Disallow: /update.php
Disallow: /UPGRADE.txt
Disallow: /xmlrpc.php
# Paths (clean URLs)
Disallow: /admin/
Disallow: /comment/reply/
Disallow: /filter/tips/
Disallow: /node/add/
Disallow: /search/
Disallow: /user/register/
Disallow: /user/password/
Disallow: /user/login/
Disallow: /user/logout/
# Paths (no clean URLs)
Disallow: /?q=admin/
Disallow: /?q=comment/reply/
Disallow: /?q=filter/tips/
Disallow: /?q=node/add/
Disallow: /?q=search/
Disallow: /?q=user/password/
Disallow: /?q=user/register/
Disallow: /?q=user/login/
Disallow: /?q=user/logout/
```


Drupal is currently version 7.57

Username for Drupal = auditor


 | Username: admin, Password: estrella
![[Pasted image 20260801114633.png]]



```
pcclient $> queryuser
Usage: queryuser rid [info level] [access mask] 
rpcclient $> lookupsids
Usage: lookupsids [sid1 [sid2 [...]]]
rpcclient $> lookupsids S-1-22-1-1000
S-1-22-1-1000 Unix User\ubuntu (1)
rpcclient $> lookupsids S-1-22-1-1001
S-1-22-1-1001 Unix User\auditor (1)
rpcclient $> lookupsids S-1-22-1-1002
S-1-22-1-1002 Unix User\dbadmin (1)

```





dbadmin-sayang


dbadmin@ip-192-168-100-52:/home/auditor$ cat flag.txt
2354da332a2444da96d6aa10a042a0f2


.50 - WINSERVER01
.51 - WINSERVER02
.55 - WINSERVER03


  'database' => 'drupal',
      'username' => 'drupal',
      'password' => 'syntex0421',
      'host' => 'localhost',
      'port' => '3306',
      'driver' => 'mysql',
      'prefix' => '',


MariaDB [(none)]> SELECT user FROM mysql.user;
+--------+
| user   |
+--------+
| root   |
| drupal |
| root   |
+--------+


[22][ssh] host: 192.168.100.52   login: auditor   password: qwertyuiop

```

1 | admin    | $S$D67i0qFmSLMLwZ9PU7VEocSS9fvV1JaSeJxQMgCid80hGbq6wXZH | admin@syntex.com     |       |           | NULL             | 1650232322 | 1650248652 | 1650248498 |      1 | America/New_York |          |       0 | admin@syntex.com     | b:0; |
|   2 | auditor  | $S$DV.wsqkmKY3y5VW.icW/g5NTU3h.UA01nxqL9Cro27GaSBYpH4WC | auditor@syntex.com   |       |           | filtered_html    | 1650234408 |          0 |          0 |      1 | America/New_York |          |       0 | auditor@syntex.com   | b:0; |
|   3 | dbadmin  | $S$DZcGD5qcb6xso1E/Mu6DJP4uPi5DfY28kBEyuIab8Pod1saBaImN | dbadmin@syntex.com   |       |           | filtered_html    | 1650248436 | 1785624771 | 1785624771 |      1 | America/New_York |          |       0 | dbadmin@syntex.com   | b:0; |
|   4 | Vincenzo | $S$DGnS.dK3q2FeWeNbLikdI5Hk/XdBFI2jBFkmPvv/v9Ln8vjIanIu | vincenzo@syntext.com |       |           | filtered_html    | 1650248490 |          0 |          0 |      1 | America/New_York |          |       0 | vincenzo@syntext.com | b:0; |
|   5 | Test     | $S$Dr7KmZXbMCfBSQryeTMJA4Ri/zUs6L5UzKam3fVhajOyp8.GoHMv | test@test.com        |       |           | filtered_html    | 1785603205 |          0 |          0 |      0 | America/New_York |          |       0 | test@test.com        | NULL |
```


Account called steven on .51 - pw is bonita for smb


```
root:$6$v8b2/P8T26uEUwvM$TBiao8o1dfqQrGPPcebRj6A6cNiixcy6/r/AFtN5Swk7N1kpg/8UyQK0pXFwdLfy5Ed/71VN91nJ6.3JyAN/00:18998:0:99999:7:::
daemon:*:18960:0:99999:7:::
bin:*:18960:0:99999:7:::
sys:*:18960:0:99999:7:::
sync:*:18960:0:99999:7:::
games:*:18960:0:99999:7:::
man:*:18960:0:99999:7:::
lp:*:18960:0:99999:7:::
mail:*:18960:0:99999:7:::
news:*:18960:0:99999:7:::
uucp:*:18960:0:99999:7:::
proxy:*:18960:0:99999:7:::
www-data:*:18960:0:99999:7:::
backup:*:18960:0:99999:7:::
list:*:18960:0:99999:7:::
irc:*:18960:0:99999:7:::
gnats:*:18960:0:99999:7:::
nobody:*:18960:0:99999:7:::
systemd-network:*:18960:0:99999:7:::
systemd-resolve:*:18960:0:99999:7:::
systemd-timesync:*:18960:0:99999:7:::
messagebus:*:18960:0:99999:7:::
syslog:*:18960:0:99999:7:::
_apt:*:18960:0:99999:7:::
tss:*:18960:0:99999:7:::
uuidd:*:18960:0:99999:7:::
tcpdump:*:18960:0:99999:7:::
sshd:*:18960:0:99999:7:::
landscape:*:18960:0:99999:7:::
pollinate:*:18960:0:99999:7:::
ec2-instance-connect:!:18960:0:99999:7:::
systemd-coredump:!!:18998::::::
ubuntu:!:18998:0:99999:7:::
lxd:!:18998::::::
rtkit:*:18998:0:99999:7:::
xrdp:!:18998:0:99999:7:::
dnsmasq:*:18998:0:99999:7:::
usbmux:*:18998:0:99999:7:::
avahi:*:18998:0:99999:7:::
cups-pk-helper:*:18998:0:99999:7:::
pulse:*:18998:0:99999:7:::
geoclue:*:18998:0:99999:7:::
saned:*:18998:0:99999:7:::
colord:*:18998:0:99999:7:::
sddm:*:18998:0:99999:7:::
gdm:*:18998:0:99999:7:::
auditor:$6$RNJCCrE9ok/yCMqD$7uPoYFsrnR3wPnSwPeLuBEiXgAzlOzGW6uZSyX.IjNNVcR5.bDBhb.dlZTN37JJR4yZXXQTetuUhOOX9ZNov6/:19099:0:99999:7:::
dbadmin:$6$1HAbXNNxXVVNCcoi$6Zy2gjvyZZYHTwSyxSLsdv0LA.5hA7EeD1WhUFzHg9SOSXrz7DxX7iG0mCQbmEBSo.yjB1c80iIujSM6Fjbpo/:19099:0:99999:7:::
mysql:!:19099:0:99999:7:::
ftp:*:19100:0:99999:7:::

```



.50 - WINSERVER01
.51 - WINSERVER02
.52 - WEBSERVER02
.55 - WINSERVER03

Use the questions to our advantage. They ask for mary's credentials on 192.168.100.55


.50 
admin - superman - smb brute force with Hydra
mike - diamond  - smb brute force with Hydra
WINSERVER-01 admin, Administrator, Guest, mike, vince

.51 
steven - bonita - smb brute force with Hydra. Found username after reading robots.txt.txt from website. 
WINSERVER-02  Administrator, Guest, steven 

.55 password
admin - blanca
mary - hotmama
Administrator - swordfish
WINSERVER-03 admin, Administrator, DefaultAccount, Guest, lawrence, mary, student, WDAGUtilityAccount 


**eJPT Flags

- Root Flag on Drupal - d4bd804774d14fda9775bb27c6f5e919


dbadmin@ip-192-168-100-52:/home/auditor$ cat flag.txt
2354da332a2444da96d6aa10a042a0f2**