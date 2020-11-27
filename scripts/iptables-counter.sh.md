---
description: >-
  For measuring incoming and outgoing traffic. Used to measure nmap packets sent
  during scan
---

# iptables-counter.sh

```bash
#!/bin/bash

#reset all counters and iptables rules
iptables -Z && iptables -F

#measure incoming traffic to 10.11.1.230
iptables -I INPUT 1 -s 10.11.1.230 -j ACCEPT

#measure outgoing traffic to 10.11.1.230
iptables -I OUTPUT 1 -d 10.11.1.230 -j ACCEPT
```



```bash
#!/bin/bash

for i in `seq 1 99999`; do

	date > iptable_nmap_UDP.log
	iptables -vn -L >> iptable_nmap_UDP.log
	sleep 5m

done

```



```bash
#!/bin/bash
iptables -Z && iptables -F
iptables -A INPUT -p tcp --destination-port 13327 \! -d 127.0.0.1 -j DROP
iptables -A INPUT -p tcp --destination-port 4444 \! -d 127.0.0.1 -j DROP


```

