# 445 tcp - SMB

_references:_ 

* [https://0xdf.gitlab.io/2018/12/02/pwk-notes-smb-enumeration-checklist-update1.html\#nmblookupAnother](https://0xdf.gitlab.io/2018/12/02/pwk-notes-smb-enumeration-checklist-update1.html#nmblookupAnother) 
* [http://www.madirish.net/59](http://www.madirish.net/59)



### SMBCLient

```bash
smbclient -L \\192.168.1.5

Enter WORKGROUP\root's password:

# Sharename       Type      Comment
#         ---------       ----      -------
#         IPC$            IPC       Remote IPC
#         share           Disk
#         wwwroot         Disk
#         ADMIN$          Disk      Remote Admin
#         C$              Disk      Default share
# Reconnecting with SMB1 for workgroup listing.
#Server               Comment
#         ---------            -------
#Workgroup            Master
#         ---------            -------
```

### NMBLookup

```bash
nmblookup -A 192.168.0.5

# Looking up status of [ip]
#         [hostname]      <00> -         M <ACTIVE>
#         [hostname]      <20> -         M <ACTIVE>
#         WORKGROUP       <00> - <GROUP> M <ACTIVE>
#         WORKGROUP       <1e> - <GROUP> M <ACTIVE>
#                         <03> -         M <ACTIVE>
#         INet~Services   <1c> - <GROUP> M <ACTIVE>
#         IS~[hostname]   <00> -         M <ACTIVE>
# MAC Address = 00-50-56-XX-XX-XX
```

### SMBClient

`smbclient -L` [`\\madirish-dt -I 192.168.0.1`](file:///madirish-dt%20-I%20192.168.0.1) `-N`

### Nmap Scan

```bash
nmap -v -p 139,445 -oG smb.txt  10.11.24.1-100

# smb enum script
nmap -p 139,445 --script smb-enum-users <ipaddress_here>

# Checks for OS of SMB
nmap -v -p 139,445 --script smb-os-discovery 10.11.24.85

#Checks for smb vulnerability
nmap -v -p 139,445 --script vuln <ipaddress_here>
```

### NBTScan

`nbtscan -r 10.11.24.0/24`

### RPCClient

_Null sessions, In windows NT2000/XP default config for SMB allows for nullsessions to be created. In windows 2003/XP SP2 onwards, this is disabled. Use RPCClient to explore nullsessions._

```bash
Rpcclient -U "" 129.168.1.200

#Server info such as OS version, server type
srvinfo

#Users information
enumdomusers

#Show password policy
getdompwinfo

#Domain info
querydomaininfo

#find net share
netshareenum

```

### Enum4Linux

`enum4linux -v 192.168.1.200`

### SMBMap

```bash
smbmap -H 192.168.1.5

 [+] Finding open SMB ports....
 [+] User SMB session establishd on [ip]...
 [+] IP: [ip]:445        Name: [ip]                                     
         Disk                                                    Permissions
         ----                                                    -----------
         ADMIN$                                                  NO ACCESS
         C$                                                      NO ACCESS
         IPC$                                                    NO ACCESS
         NETLOGON                                                NO ACCESS
         Replication                                             READ ONLY
         SYSVOL                                                  NO ACCESS
```



