---
description: forward DNS query bruteforce
---

# \(DNS\) dns-fwd-brute.sh

```bash
#!/bin/bash
#Forward DNS query bruteforce

for name in $(cat list.txt); do
	host $name.megacorpone.com | grep "has address" | cut -d" " -f1,4
done
```

