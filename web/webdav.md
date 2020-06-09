# Webdav

### Nmap scan Webdav

`nmap -p80 --script=http-iis-webdav-vuln 1.1.1.1`

### DavTest

```bash
# davtest detect vulnerability to upload
davtest -url http://1.1.1.1

# davtest uploading files
davtest -url http://target.com -uploadfile '/my/directory/file.html' -uploadloc exploit.html
```

### Webdav commands

```bash
# Copy a resource from one URI to another
COPY

# Put resource 
PUT

# Change and delete multiple properties on a resource in a single atomic act
PROPPATCH 

PROPFINDetrieve properties, stored as XML, from a web resource. It is also overloaded to allow one to retrieve the collection structure (a.k.a. directory hierarchy) of a remote system.

# Remove a lock from a resource
UNLOCK

# Put a lock on a resource. WebDAV supports both shared and exclusive locks.
LOCK 

#used to create collections (a.k.a. a directory)
MKCOL

```

\*\*\*\*

