---
title: Active Directory
nav_order: 2
sidebar: auto
permalink: /ad/
---

## Quick summary
Active Directory (AD) is Microsoft’s directory service for Windows domain networks. This cheat-sheet focuses on the offensive/defensive primitives most useful during CTFs and OSCP-style labs: enumeration, Kerberos, privilege escalation, and post-exploitation.


## One-page mental map
- **Domain Controller (DC)** — stores AD database, authenticates users
- **Kerberos** — primary auth protocol; look for SPNs
- **LDAP** — directory queries for users/groups
- **SMB** — shares, GPOs, sysvol


## Common ports
- 88 — Kerberos
- 389 — LDAP
- 636 — LDAPS
- 445 — SMB
- 3268 — Global Catalog (LDAP)
