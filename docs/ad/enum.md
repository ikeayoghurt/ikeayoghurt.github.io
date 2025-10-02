---
title: AD Recon — Initial Enumeration
sidebar: auto
permalink: /ad/recon/initial-enumeration/
---

# Initial Enumeration — Active Directory (AD)

**Goal:** quick first-pass commands to discover hosts, domain controllers, DNS records and gather targets for deeper AD enumeration. Keep this as your first-run checklist when you acquire network access or a foothold.

---

# Key Data Points
| Data Point | Description |
|---|---|
| AD Users | We are trying to enumerate valid user accounts we can target for password spraying. |
| AD Joined Computers | Key Computers include Domain Controllers, file servers, SQL servers, web servers, Exchange mail servers, database servers, etc. |
| Key Services | Kerberos, NetBIOS, LDAP, DNS |
| Vulnerable Hosts and Services | Anything that can be a quick win. ( a.k.a an easy host to exploit and gain a foothold) |


# Enumerating without Credentials

## Enumeration services on host
### Nmap
```bash
nmap -sC -sV <ip> -oA <filename>
```
> General example, `-sC` default scripts, `-sV` enumerate version, `-oA` output in all formats

Useful flags:
- `-sU` UDP Scan
- `-T4` aggressive (fast scan)
- `-sS` "Stealth" scan



## Enumerating using services
### SMB (445)
Anonymous
```bash
netexec smb -u '' -p '' <optional flags>
```
Guest
```bash
netexec smb -u 'a' -p '' <optional flags>
```

**Useful flags:**
- `--shares`
- `--rid-brute`
- `--users`
- `--groups`
- `--pass-pol`
