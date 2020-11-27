---
description: Reverse DNS lookup bruteforce
---

# \(DNS\) dns-rev-brute.sh

```bash
#!/bin/bash
#Reverse DNS lookup bruteforce example

for ip in $(seq 71 84); do
	host 38.100.193.$ip | grep "megacorp" | cut -d" " -f1,5
done

```

