# ping sweep

## ping-sweep.py

```python
#!/usr/bin/python

import ipaddress
import os


#ip address string must be in unicode to use ipaddress module
network = ipaddress.ip_network(unicode("10.11.1.0/24"))

print("Running Python ping sweep of target IP range 10.11.1.0/24")

for i in network.hosts():
	#str(i)
	
	response = os.system("ping %s -c 1 > /dev/null" %i)
	
        if response == 0:
                print("%s UP" %i)
        else:
                print("%s No response" %i)

print("Ping sweep complete")

```

## powersweep.py

multi-threaded

```python
#!/usr/bin/python

import multiprocessing
import subprocess
import os

def pinger( job_q, results_q ):
    DEVNULL = open(os.devnull,'w')
    while True:
        ip = job_q.get()
        if ip is None: break

        try:
            subprocess.check_call(['ping','-c1',ip],
                                  stdout=DEVNULL)
            results_q.put(ip)
        except:
            pass

if __name__ == '__main__':
    pool_size = 255

    jobs = multiprocessing.Queue()
    results = multiprocessing.Queue()

    pool = [ multiprocessing.Process(target=pinger, args=(jobs,results))
             for i in range(pool_size) ]

    for p in pool:
        p.start()

    for i in range(1,255):
        jobs.put('10.11.1.{0}'.format(i))

    for p in pool:
        jobs.put(None)

    for p in pool:
        p.join()

    os.system("touch results.txt")

    while not results.empty():
        ip = results.get()
        print(ip)


```

## pingsweep.sh

```bash
#!/bin/bash

echo "Running Bash loop to perform a ping sweep of target IP range 10.11.1.0/24"

IP=10.11.1.

for x in `seq 1 254`; do
ping -c 1 $IP$x | grep "64 bytes" | cut -d " " -f 4 | cut -d ":" -f 1
done

echo "Ping sweep complete"
```



