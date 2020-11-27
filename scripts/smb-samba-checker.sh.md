---
description: enum4linux messed up and doesnt report samba version. Thanks for this!
---

# \(SMB\) samba-checker.sh

```bash
#!/bin/sh
#Author: rewardone
#Thanks fellow student OS-40285!
#
#Description:
# enum4linux messed up and doesnt report samba version.
# 
# Requires root or enough permissions to use tcpdump
# Will listen for the first 7 packets of a null login
# and grab the SMB Version
#Notes:
# Will sometimes not capture or will print multiple
# lines. May need to run a second time for success.
if [ -z $1 ]; then echo "Usage: $0 <ipaddress> <port>" && exit; else rhost=$1; fi 
if [ ! -z $2 ]; then rport=$2; else rport=139; fi
tcpdump -s0 -n -i tap0 src $rhost and port $rport -A -c 7 2>/dev/null | grep -i "samba\|s.a.m" | tr -d '.' | grep -oP 'UnixSamba.*[0-9a-z]' | tr -d '\n' & echo -n "$rhost: " & 
echo "exit" | smbclient -L $rhost 1>/dev/null 2>/dev/null
echo "" && sleep .1

```

