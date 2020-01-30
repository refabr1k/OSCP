# SSRF

### Testing for SSRF vuln

You can get server to request for a url and load resources from it. Eg. If we use the api end point to browse to a webserver, we are able to load it successfully \(it is not the same as RFI\).

![](.gitbook/assets/image%20%283%29.png)

To confirm if you have SSRF you should be able to 'query' internal network such as local host ports.

![](.gitbook/assets/image%20%284%29.png)

### SSRF scanning local ports \(use --hl=2 to hide Lines that return 2 only.. This is if

there are too many false positives\) wfuzz -c -z range,1-65535 --hl=2 [http://10.10.10.55:60000/url.php?path=127.0.0.1:FUZZ](http://10.10.10.55:60000/url.php?path=127.0.0.1:FUZZ)

![](.gitbook/assets/image%20%285%29.png)

We are able to query internal ports 22, 90,110,200,320,888 To view these resources simply browse to them. Or use burp suite

