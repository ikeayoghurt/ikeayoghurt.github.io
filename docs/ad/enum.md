---
title: Initial Enumeration
sidebar: auto
parent: Active Directory
---

# Initial Enumeration — Active Directory (AD)
Enumerating an AD environment can be overwhelming due to the large surface area, the key will be to get a lay of the land and honing in potential avenues for a foothold.


# Enumerating without Credentials
Without creds, you may be pretty limited! Let us look at useful tips to getting you that first set of creds.

## Enumerating services on host

### Nmap
```bash
# General example, `-sC` default scripts, `-sV` enumerate version, `-oA` output in all formats
nmap -sC -sV <ip> -oA <filename>
```

Useful flags:
- `-sU` UDP Scan
- `-T4` aggressive (fast scan)
- `-sS` "Stealth" scan


## Enumerating without creds
### LLMNR/NBT-NS Poisioning
```bash
# Running some responder in the background and who knows what could happen?
sudo responder -I <tun>
```

### HTTP/HTTPS (80/443)
```
# Check the site you numpty
## No more site? Is there vhost or subdomains?
```

### RPC (135)
```bash
# Check NULL session
rpcclient -U "" -N <IP>
```

```bash
# Enum domain users
rpclient> enumdomusers
```


### SMB (445)
```bash
# NULL session below, also check with guest access -u 'a'
netexec smb -u '' -p '' <optional flags> --users <--rid-brute> <--pass-pol>
smbclient -L \\test.local -I <IP> -N
```
If you managed to have NULL or guest access, connect and have a look around
```bash
# List the shares
smbclient \\\\test.local\\<share> -I <IP> -N
impacket-smbclient 'domain/user@<ip>' # might need to fix this up...
```


### LDAP
```bash
# Something witty to put here?
ldapsearch -LLL -x -H ldap://test.local -b'' -s base '(objectclass=\*)'
```

```bash
# You got lucky with null access? Maybe bloodhound can show you more
bloodhound-python-ce -d test.local -v --zip -c All -dc test.local -ns 10.10.10.1
```

### Kerberos
```bash
# Must of run out of leads... brute force time
kerbrute userenum -d test.local usernames.txt
```

### I'm feeling lazy or need a hand
```bash
# Sometimes it's best to let the script enum if your brain gone mushy and missed something
enum4linux -a <ip>
enum4linux-ng <ip> #the difference between the two? Need to check...
```



# Returning to the below later - WIP

## Enumerating the services
### SMB (445)
```bash
netexec smb -u '' -p '' <optional flags>
```
> Check Anonymous

```bash
netexec smb -u 'a' -p '' <optional flags>
```
> Check Guest

**Useful flags:**
- `--shares`
- `--rid-brute`
- `--users`
- `--groups`
- `--pass-pol`



### LMNR/NBT-NS Poisoning
```bash
sudo responder -I <tun>
```
> Linux (using Responder)

```powershell
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
> Windows (using Inveigh)

At times may find web applications with functionality to connect to arbitary IPs over certain services. Try send a request to your attacker box with `Responder` running - you might get lucky with a user hash.
> Refer `Walkthrough` on HacktheBox


### Password Spraying
1. Before you spray, atleast try get a list known list of users and the password policy from SMB, LDAP, RPC.
