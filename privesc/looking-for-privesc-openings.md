# Enable RDP @ Firewall

## Method 1

```c
netsh firewall set service RemoteDesktop enable
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
reg add "hklm\system\currentControlSet\Control\Terminal Server" /v "AllowTSConnections" /t REG_DWORD /d 0x1 /f
sc config TermService start= auto
net start Termservice
netsh.exe

Firewall
add portopening TCP 3389 "Remote Desktop"
```

## Method 2

```c
# TCP 
netsh.exe advfirewall firewall add rule name="Remote Desktop - User Mode (TCP-In)" dir=in action=allow 
program="%%SystemRoot%%\system32\svchost.exe" service="TermService" description="Inbound rule for the Remote Desktop service to allow RDP traffic. [TCP 3389] added by LogicDaemon's script" enable=yes 
profile=private,domain localport=3389 protocol=tcp

# UDP
netsh.exe advfirewall firewall add rule name="Remote Desktop - User Mode (UDP-In)" dir=in action=allow 
program="%%SystemRoot%%\system32\svchost.exe" service="TermService" description="Inbound rule for the 
Remote Desktop service to allow RDP traffic. [UDP 3389] added by LogicDaemon's script" enable=yes 
profile=private,domain localport=3389 protocol=udp

```

## Method 3 \(metasploit\)

```bash
# Enable RDP (metasploit)
run post/windows/manage/enable_rdp
https://www.offensive-security.com/metasploit-unleashed/enabling-remote-desktop/ 
```

