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

## Juicy files

```bash
/etc/*issue
/etc/*release
/proc/version
/etc/profile
/etc/passwd
/etc/shadow
/root/.bash_history
/var/log/dmessage
/var/mail/root
/var/spool/cron/crontabs/root


# can see what is the PID or ID of running process, correspond to /etc/passwd see if you can tell which user
/proc/self/status

# Can check what 'user agents', if you have access you may be able to do code execution modifying 'user agents'
/proc/self/environ

# Anything blocking us from bruteforcing?
etc/pam.d/system-auth
etc/fail2ban/fail2ban.conf


# OSX / macOS
/etc/fstab
/etc/master.passwd 
/etc/resolv.conf
/etc/sudoers
/etc/sysctl.conf
```



