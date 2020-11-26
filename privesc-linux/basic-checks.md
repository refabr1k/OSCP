# Basic checks

## OS version

```bash
# Check OS version
cat /etc/issuecat 
/etc/*-releasecat 
/etc/lsb-release      # Debian based
cat /etc/redhat-release   # Redhat based
```

## Kernel version

```text
cat /proc/version
uname -a
uname -mrs
rpm -q kernel
dmesg | grep Linux
ls /boot | grep vmlinuz-
```

## Environment Variables

```text
cat /etc/profile
cat /etc/bashrc
cat ~/.bash_profile
cat ~/.bashrc
cat ~/.bash_logout
env
set

```

## Find

```bash
#World writable folders
find / -writable -type d 2>/dev/null -exec ls -ldb {} \;

#Root suid files
find / -user root -perm -4000 -type f 2>/dev/null -exec ls -ldb {} \;

```

