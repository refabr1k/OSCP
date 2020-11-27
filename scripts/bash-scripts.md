---
description: Bash scripts from elearn security course - ejpt
---

# Bash scripts

## getting port numbers from nmap scans

```bash
cat *.nmap | grep "open" | grep -v "filtered" | cut -d "/" -f 1 | sort -u | xargs | tr ' ' ',' > ports.txt

cat ports.txt
# 21,22,25,80,110,143,443,1433
```

## Get http response codes using curl

```bash
#curl script to get http response
curl -L --write-out "%{http_code}\n" --output /dev/null --silent --insecure http://hello.expiredssl.com
```

## read a list of domains and append http/https

```bash
# cat domain.txt
# 127.0.0.1:1337
# www.google.com.sg

# myscript.sh
#!/bin/bash
while read line;
do
    echo $line
done < domains.txt

# ./myscript.sh
# 127.0.0.1:1337
# www.google.com.sg
```

```bash
# myscript.sh
#!/bin/bash
for protocol in 'http://' 'https://';do
    while read line;
    do
        echo $protocol$line
    done < domains.txt
done

#./myscript.sh
# http://127.0.0.1:1337
# http://www.google.com.sg
# https://127.0.0.1:1337
# https://www.google.com.sg
```

```bash
#!/bin/bash

for protocol in 'http://' 'https://'; do
    while read line;
    do
        code=$(curl -L --write-out "%{http_code}\n" --output /dev/null --silent --insecure $protocol$line)
        if [ $ code = "000" ]; then
            echo "$protocol$line: not responding"
        else
            echo "$protocol$line: HTTP $code"
            echo "$protocol$line: $code" >> alive.txt

        done < domains.txt
done

#./myscript.sh
# http://127.0.0.1:1337: not responding.
# http://www.google.com.sg: HTTP 200
# https://127.0.0.1:1337: not responding.
# https://www.google.com.sg. HTTP 200

# cat alive.txt
# http://www.google.com.sg: HTTP 200
# https://www.google.com.sg. HTTP 200
```

