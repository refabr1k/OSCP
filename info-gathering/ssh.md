# T:22 - SSH

### [OpenSSH 2.3 &lt; 7.7 - Username Enumeration](https://www.exploit-db.com/exploits/45233)

`python openssh_username_enum.py --port 22 --userList ssh_list.txt --outputFile ssh_enum_results.txt 10.10.10.55`

#### Nmap nse script

```text
nmap -sV --script="ssh* and note brute" -p22 <target-ip> -oN ssh_script_scan.nmap
```



