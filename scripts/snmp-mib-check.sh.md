---
description: wrapper for snmpwalk
---

# \(SNMP\) mib-check.sh

```bash
#!/bin/bash
# simple script to run snmp walk against a list of mib values
#
# mib values provided in file should be in such format: eg.
# 1.3.6.1.4.1.77.1.2.25 User Accounts
# 1.3.6.1.2.1.6.13.1.3 TCP Local Ports

if [ -z "$1" ] || [ -z "$2" ] || [ -z "$3" ]
then
echo "Usage: ./snmpwalk_mib_list.sh <ipaddress> <community> <mib list>" 
echo "example: ./snmpwalk_mib_list.sh 192.168.1.1 public miblist.txt"
echo ""
echo "note: mib list should be in such format eg."
echo "1.3.6.1.4.1.77.1.2.25 User Accounts"
echo "1.3.6.1.2.1.6.13.1.3 TCP Local Port"
else

IFS=$'\n'       # make newlines the only separator

for x in $(cat $3); do
	echo "snmpwalking "$1 "and Checking for mib value: "$x
	snmpwalk -c $2 -v1 $1 $(echo $x | cut -d' ' -f1)
	echo ""
done
fi


```

