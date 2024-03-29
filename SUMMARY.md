# Table of contents

* [refabr1k's Pentest Notebook](README.md)
* [Steganography](steganography.md)
* [Kali USB with persistence memory](kali-usb-with-persistence-memory.md)
* [useful tools](useful-tools.md)
* [Understanding ICACLS permissions](understanding-icacls-permissions.md)

## INFO GATHERING

* [Port Knocking](info-gathering/port-knocking.md)
* [22 tcp - SSH](info-gathering/ssh/README.md)
  * [SSH Tunneling](info-gathering/ssh/ssh-tunneling.md)
* [25 tcp - SMTP](info-gathering/smtp.md)
* [53 tcp/udp - DNS](info-gathering/dns.md)
* [88 tcp - Kerberos](info-gathering/t-88-kerberos.md)
* [161 udp - SNMP](info-gathering/snmp.md)
* [445 tcp - SMB](info-gathering/smb.md)
* [1098,1099 tcp - Java RMI](info-gathering/t-1098-1099-java-rmi.md)
* [8009 tcp - AJP](info-gathering/t-8009-ajp.md)
* [5901,5902 tcp - VNC](info-gathering/untitled.md)

## Web

* [XSS cookie stealing](web/xss.md)
* [PHP](web/php.md)
* [Webdav](web/webdav.md)
* [Wordpress](web/wordpress.md)
* [XML RPC](web/xml-rpc.md)
* [SQL Injection](web/sql-injection.md)
* [SSRF](web/ssrf.md)

## EXPLOITATION <a id="exploitation-1"></a>

* [File Transfers](exploitation-1/file-transfers.md)
* [Buffer Overflow](exploitation-1/buffer-overflow.md)
* [Bruteforce](exploitation-1/bruteforce/README.md)
  * [Hashcat](exploitation-1/bruteforce/hashcat.md)
  * [Ophcrack \(rainbow tables\)](exploitation-1/bruteforce/ophcrack-rainbow-tables.md)
  * [John The Ripper](exploitation-1/bruteforce/john-the-ripper.md)
* [PHP rce](exploitation-1/php-rce.md)
* [Compiling](exploitation-1/compiling-code.md)
* [msfvenom](exploitation-1/untitled-1.md)
* [Reverse shell](exploitation-1/reverse-shell.md)
* [Using ENV to escape Bad Characters](exploitation-1/using-env-to-escape-bad-characters.md)
* [shellshock](exploitation-1/shellshock.md)
* [Ncat Persistent Backdoor](exploitation-1/ncat-persistent-backdoor.md)

## PRIVESC - LINUX

* [Basic checks](privesc-linux/basic-checks.md)
* [Upgrading Shells](privesc-linux/upgrading-shells.md)
* [SUID](privesc-linux/suid.md)

## Privesc - Windows <a id="privesc"></a>

* [Basic checks / powershell](privesc/powershell-cmd.md)
* [Privesc Openings](privesc/privesc-openings.md)
* [LonelyPotato - SeImpersonatePrivilege](privesc/lonelypotato-seimpersonateprivilege.md)
* [Enable RDP @ Firewall](privesc/looking-for-privesc-openings.md)
* [NTLM \(Pass The Hash\)](privesc/ntlm-pass-the-hash.md)

## Windows

* [NTDS.dit](windows/ntds.dit.md)
* [Responder / SMB Relay](windows/active-directory-attacks.md)
* [Attacking AD](windows/attacking-ad/README.md)
  * [AD Hacking Lab Setup](windows/attacking-ad/ad-hacking-lab-setup.md)

## Metasploit

* [Basic Usage](metasploit/basic-usage.md)
* [Meterpreter](metasploit/meterpreter/README.md)
  * [Pivoting](metasploit/meterpreter/pivoting.md)
  * [Windows Post Exploitation](metasploit/meterpreter/windows-post-exploitation.md)

## Unsorted

* [other notes](unsorted/other-notes.md)

## eLearnSecurity eJPT

* [eJPT notes](elearnsecurity-ejpt/untitled.md)

## OSWP

---

* [Getting started](oswp-notes.md)
* [WEP Attacks](wep-attacks/README.md)
  * [WEP Attack \(OPEN\) - Clients connected](wep-attacks/wep-attack-clients-connected.md)
  * [WEP Attack  \(OPEN\) - Clientless](wep-attacks/wep-attack-clientless.md)
  * [WEP Attack \(SKA\)](wep-attacks/wep-attack-ska.md)
* [WPA/WPA2 Attacks](wpa-wpa2-attacks.md)

## Scripts

* [get port from nmap](scripts/get-port-from-nmap.md)
* [Curl response](scripts/bash-scripts.md)
* [ping sweep](scripts/ping-sweep.md)
* [iptables-counter.sh](scripts/iptables-counter.sh.md)
* [\(DNS\) zonetransfer\_check.sh](scripts/dns-zonetransfer_check.sh.md)
* [\(DNS\) dns-rev-brute.sh](scripts/dns-dns-rev-brute.sh.md)
* [\(DNS\) dns-fwd-brute.sh](scripts/dns-dns-fwd-brute.sh.md)
* [\(SMB\) vuln-scan.sh](scripts/smb-vuln-scan.sh.md)
* [\(SMB\) samba-checker.sh](scripts/smb-samba-checker.sh.md)
* [\(SMTP\) vrfy.py](scripts/smtp-vrfy.py.md)
* [\(SNMP\) mib-check.sh](scripts/snmp-mib-check.sh.md)

## Zeroday vulnerabilities explained

* [2020-12 Solarwind supply chain](zeroday-vulnerabilities-explained/solarwind.md)

