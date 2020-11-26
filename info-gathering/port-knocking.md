# Port Knocking

### Nmap TCP Port knock in sequence

```bash
IFS=$(' ')

#knock in sequence
for i in 324 125 235;do nmap -Pn -p $i --host-timeout 201 --max-retries 0 10.10.10.23; done

#after knock check if port is opened
nmap -p 22 10.10.10.23
```



