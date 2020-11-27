# get port from nmap

## getting port numbers from nmap scans

```bash
cat *.nmap | grep "open" | grep -v "filtered" | cut -d "/" -f 1 | sort -u | xargs | tr ' ' ',' > ports.txt

cat ports.txt
# 21,22,25,80,110,143,443,1433
```

