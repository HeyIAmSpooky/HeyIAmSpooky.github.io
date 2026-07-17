---
title: Metasploit Framework
dg-publish: true
---

# Metasploit Framework

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#Metasploit Framework CLI|Metasploit Framework CLI]]

---

## Metasploit Framework CLI
The **Metasploit Framework Command Line Interface (MSFcli)** is a command line utility used to facilitate the creation of automation scripts that utilize Metasploit modules. It can redirect output from other tools into msfcli and vice versa.

## External Sources
- [Metasploit Documentation Home](https://docs.metasploit.com/)
- [Metasploit Documentation - Modules](https://docs.metasploit.com/docs/modules.html)

## Visual Diagram
```mermaid
flowchart LR
A[search]-->B[use]
B-->C[show options]
C-->D[set options]
D-->E[run/exploit]
E-->F[sessions]
```

## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]

---
## Migrated from Unsorted Notes — Metasploit Community Edition

### Metasploit Community Edition
A web-based GUI front-end for the Metasploit Framework that simplifies network discovery and vulnerability identification.

---
## Migrated from Unsorted Notes — Armitage

### Armitage
A free Java-based GUI front-end for the Metasploit Framework that simplifies network discovery, exploitation, and post-exploitation.

---
## Migrated from Unsorted Notes — MSF Architecture

### MSF Architecture

```
TOOLS ──────────► Rex (library)
                     │
PLUGINS ────────► MSF Base ──► MSF Core ──► INTERFACES
                     │                          ├─ MSFconsole
MODULES ─────────────┘                          ├─ MSFcli
  ├─ EXPLOIT                                     ├─ Armitage
  ├─ PAYLOAD                                     └─ Web
  ├─ ENCODER
  ├─ NOP
  └─ AUXILIARY
```

---
## Migrated from Unsorted Notes — MSF Modules

### MSF Modules
- **Exploit** — Takes advantage of a vulnerability, typically paired with a payload.
- **Payload** — Code delivered by MSF and remotely executed on the target after successful exploitation (e.g., a reverse shell initiating a connection from the target back to the attacker).
- **Encoder** — Used to encode payloads to avoid AV detection (e.g., `shikata_ga_nai` for Windows payloads).
- **NOPS** — Ensures payload sizes are consistent and stable when executed.
- **Auxiliary** — Performs additional functionality like port scanning and enumeration.

---
## Migrated from Unsorted Notes — Meterpreter Payload

### Meterpreter Payload
The Meterpreter (Meta-Interpreter) payload is an advanced multi-functional payload executed in memory on the target system, making it difficult to detect. It communicates over a stager socket and provides an interactive command interpreter that facilitates system command execution, file system navigation, keylogging, and more.

---
## Migrated from Unsorted Notes — Penetration Testing with MSF

### Penetration Testing with MSF

MSF can be used to perform and automate various tasks under the penetration testing life cycle. We can adopt PTES (Penetration Testing Execution Standard) as a roadmap:

```
Penetration Testing Phases
    │
    ├─ Information Gathering
    ├─ Enumeration
    ├─ Exploitation
    └─ Post Exploitation
         ├─ Privilege Escalation
         ├─ Maintaining Persistent Access
         └─ Clearing Tracks
```

---
