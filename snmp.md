# SNMP

### NMAP Scan

`nmap -sU --open -p 161 10.11.1.1-254 -oG mega-snmp.txt`

### SNMP Walk

```bash
snmpwalk -c public -v1 10.11.1.219
iso.3.6.1.2.1.1.1.0 = STRING: "Linux ubuntu 3.2.0-23-generic #36-Ubuntu SMP "
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10
iso.3.6.1.2.1.1.3.0 = Timeticks: (66160) 0:11:01.60
```



### SNMP Community Strings

`/usr/share/metasploit-framework/data/wordlists/snmp_default_pass.txt`



### SNMPWALK

```bash
snmpwalk -c <communityString> <ipAddress> <mibValue>
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.4.1.77.1.2.25

#Enumerating Windows Users
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.4.1.77.1.2.25

#Enumerating Running Windows Processes
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.25.4.2.1.2

#Enumerating Open TCP Ports
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.6.13.1.3

#Enumerating Installed Software
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.25.6.3.1.2

#Enumerating Windows Users
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.4.1.77.1.2.25

#Enumerating Running Windows Processes
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.25.4.2.1.2

#Enumerating Open TCP Ports
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.6.13.1.3

#Enumerating Installed Software
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.25.6.3.1.2
```

### Juicy MIB Values

```bash
# MIB Value        Microsoft Windows SNMP parameters
1.3.6.1.2.1.25.1.6.0         System Processes
1.3.6.1.2.1.25.4.2.1.2         Running Programs
1.3.6.1.2.1.25.4.2.1.4         Processes Path
1.3.6.1.2.1.25.2.3.1.4         Storage Units
1.3.6.1.2.1.25.6.3.1.2         Software Name
1.3.6.1.4.1.77.1.2.25         User Accounts
1.3.6.1.2.1.6.13.1.3         TCP Local Ports
```

### Snmp-check

`snmp-check 10.11.1.5 -c public`

### OneSixtyOne - Bruteforcing community strings

`onesixtyone -c ListOfcommunity.txt -i ListOfIps.txt`

