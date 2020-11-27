---
description: SMTP enum user
---

# \(SMTP\) vrfy.py

```python
#!/usr/bin/python

import socket
import sys

if len(sys.argv) != 3:
	print "Usage: vrfy.py <username> <ipaddress>"
	sys.exit(0)

print "Verifying user: " + sys.argv[1] + " with " + sys.argv[2]
try:
	s=socket.socket(socket.AF_INET, socket.SOCK_STREAM) #create a socket
	connect=s.connect((sys.argv[2],25)) #connect to the server
	banner=s.recv(1024)
	print banner
	s.send('VRFY ' + sys.argv[1] + '\r\n') #VRFY a user
	result=s.recv(1024)
	print "There is some response: "
	print result
except:
	print "Unable to verify. Server maybe offline/port filtered/unopened"
finally:
	s.close() #close the socket)

```

