# Powershell / Cmd

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
      <td style="text-align:left">C:\Windows\SysNative\WindowsPowershell\v1.0\powershell.exe</td>
      <td style="text-align:left">Powershell default path IMPORTANT: Using SysNative will get us to use
        the correct Powershell (32bit or 64bit) version, if we do not use absolute
        path the 32bit powershell will be used instead - this would be cause problems
        if you are trying to run privesc exploits in powershell later on a 64bit
        machine.</td>
    </tr>
    <tr>
      <td style="text-align:left">Get-Process &lt;process name&gt; -FileVersionInfo</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>



