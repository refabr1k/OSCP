# Basic checks / powershell

## Default powershell path

Powershell default path IMPORTANT: Using SysNative will get us to use the correct Powershell \(32bit or 64bit\) version, if we do not use absolute path the 32bit powershell will be used instead - this would be cause problems if you are trying to run privesc exploits in powershell later on a 64bit machine.

```text
C:\Windows\SysNative\WindowsPowershell\v1.0\powershell.exe
```

## Basic

<table>
  <thead>
    <tr>
      <th style="text-align:left">Syntax</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>systeminfo<br /></code>
      </td>
      <td style="text-align:left">System info, os, achitecture</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>hostname<br /></code>
        </p>
        <p><code>echo %username%</code>
        </p>
      </td>
      <td style="text-align:left">Hostname/compute rname</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>net user &lt;username&gt;<br /></code>
        </p>
        <p><code>Whoami /priv</code>
        </p>
      </td>
      <td style="text-align:left">user privileges</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>net localgroups</code>
      </td>
      <td style="text-align:left">Groups</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>net localgroup administrators</code>
      </td>
      <td style="text-align:left">Admin group users</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>set<br /></code>
        </p>
        <p><code>Get-ChildItem Env: | ft Key,Value</code>
        </p>
      </td>
      <td style="text-align:left">Env variables</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Get-Process &lt;process name&gt; -FileVersionInfo</code>
      </td>
      <td style="text-align:left">Process</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Get-ChildItem -Path &quot;C:\Users\Folder&quot; -Recurse -File | Select-String &quot;Password&quot;</code>
      </td>
      <td style="text-align:left">Recursive search string</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>powershell.exe -exec bypass -Command &#x201C;&amp; command1; command2&quot;}</code>
      </td>
      <td style="text-align:left">Powershell execute from cmd</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>cmd /c &quot;ftp -v -n -s:ftp.txt&quot;</code>
      </td>
      <td style="text-align:left">FTP</td>
    </tr>
  </tbody>
</table>

## Network

<table>
  <thead>
    <tr>
      <th style="text-align:left">Syntax</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>ipconfig /all<br /></code>
      </td>
      <td style="text-align:left">Ip
        <br />address</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>route print<br /></code>
      </td>
      <td style="text-align:left">
        <br />Route
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>arp -A<br /></code>
      </td>
      <td style="text-align:left">Arp (other machines that ping it)
        <br />
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>netstat -ano<br /></code>
      </td>
      <td style="text-align:left">Open ports</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>netsh firewall show state<br /></code>
        </p>
        <p><code>netsh firewall show config<br /></code>
        </p>
      </td>
      <td style="text-align:left">Check Firewall</td>
    </tr>
  </tbody>
</table>

## Juicy Info

### Reg query passwords in registry

```c
# Passwords in registry
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"
reg query HKEY_LOCAL_MACHINE\SOFTWARE\RealVNC\WinVNC4 /v password

```

### Dir search password files

```c
# Find writeable files
dir /a-r-d /s /b

# Password files
dir /b /s unattend.xml
dir /b /s web.config
dir /b /s sysprep.inf
dir /b /s sysprep.xml
dir /b /s *pass*
dir /b /s vnc.ini
dir /s *pass* == *cred* == *vnc* == *.config*
```

### Using 'findstr' to find files

```c
# findstr syntax
findstr /si password *.xml *.ini *.txt
```

### Juicy files to look

#### Plaintext or base64 encoded passwords / config files

```c
C:\sysprep.inf
C:\sysprep\sysprep.xml
%WINDIR%\Panther\Unattend\Unattended.xml
%WINDIR%\Panther\Unattended.xml 
%SYSTEMROOT%repairsystem
%SYSTEMROOT%repairSAM
%SYSTEMROOT%repairSAM
%WINDIR%win.ini
%SYSTEMDRIVE%boot.ini
%WINDIR%Panthersysprep.inf
%WINDIR%system32configAppEvent.Evt

```

#### Encrypted password in plaintext. Can use PowerSploit's Get-GPPPassword module

```c
Groups.xml
```

| Syntax | Description |
| :--- | :--- |


