---
description: 'nmap vuln script for 139,445'
---

# \(SMB\) vuln-scan.sh

```bash
#!/bin/bash
for server in $(grep microsoft nmap_lab_open_SMB.txt | cut -d" " -f2); do nmap -p 139,445 --script vuln -oN nmap_lab_SMB_win_vuln.txt --append-output $server; done

```

