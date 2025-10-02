---
title: AD — Initial Enumeration
sidebar: auto
permalink: /ad/initial-enumeration/
---

# Initial Enumeration — Active Directory (AD)
Enumerating an AD environment can be overwhelming due to the large surface area, the key will be to get a lay of the land and honing in potential avenues for a foothold.


# Enumerating without Credentials
Without creds, you may be pretty limited! Let us look at useful tips to getting you that first set of creds.

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
