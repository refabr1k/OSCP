# Notes

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

