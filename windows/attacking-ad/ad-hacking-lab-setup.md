---
description: Cybermentor
---

# AD Hacking Lab Setup

Description: A Beginners guide to hacking windows Active Directory. This is a follow along guide and notes for Cyber Mentor's udemy course https://www.udemy.com/course/practical-ethical-hacking/ Highly recommended to sign up \(with a small fee\) as he always gives out discount coupons!

Credits to:

* Heath Adams \(CyberMentor\) Pentesting udemy course - [https://www.udemy.com/course/practical-ethical-hacking/](https://www.udemy.com/course/practical-ethical-hacking/)

  %20Resources/Active%20Directory%20Attack.md

## Setting up lab

Using VMWare workstation 15 player

* 1 x Win Server 2019 _\(Domain controller\)_
* 1 x Win 10 Enterprise - _User machine 1_
* 1 x Win 10 Enterprise - _User machine 2_
* 1 x Kali linux - _Attacker_

  \(all machines would be about 2gb ram each = total 8gb ram\)

Download the required iso from [https://www.microsoft.com/en-us/evalcenter/](https://www.microsoft.com/en-us/evalcenter/)

## VM Setup

remove floppy drives network: NAT Windows server installation:

**Domain Controller** \(OS Installation\)

* Create new virtual machine
* Select iso for installer disc image
* Select version to install 'Windows Server 2019'
* [ ] Power on this virtual machine after creation
* VM Settings &gt; remove floppy disk
* Network NAT
* Start VM and press any key to enter setup mode
* Select 'Windows Sever 2019 Standard Eval \(Desktop Experience\)'
* Select 'Custom install' &gt; select unallocated space &gt; Next

\(Setting up Domain Controller\)

* Settings &gt; View Your PC Name &gt; Rename this PC &gt;  eg. `myDomainController` and restart
* Set any password eg. `P@$$w0rd`
* Player &gt; Manage &gt; Install VMWare Tools
* Server Manager &gt; Dashboard &gt; Manage &gt; Add Roles and Features &gt; Role-based install or feature-based Installation &gt; \[x\] Active Directory Domain Services &gt; Next &gt; Install
* Server Manager &gt; Flag icon &gt; 'Promote this server to a domain controller'
* Add a new forest &gt; Root domain name eg. `ADHACKING.local` &gt; Next
* Give DSRM a password eg. `P@$$w0rd` &gt; Next for all &gt; Install

**User machine** \(OS Installation\)

* Create new virtual machine
* Select iso for installer disc image
* Select version to install 'Windows 10 Enterprise'
* [ ] Power on this virtual machine after creation
* VM Settings &gt; remove floppy disk
* Network NAT
* Start VM and press any key to enter setup mode
* Select 'Custom install' &gt; select unallocated space &gt; Next
* Select 'Domain join instead' &gt; Enter acc name eg. `saul goodman` &gt; password `Password1`
* No for 'Do more across devices...'
* Decline for 'Get help from digital assistant'
* [ ] all for privacy settings

\(Setting up User machine\)

* Player &gt; Manage &gt; Install VMWare Tools
* Settings &gt; View Your PC Name &gt; Rename this PC &gt;  eg. `Saul-PC` and restart

Note: Complete these steps again for the 2nd machine account name: `walter white` pass:`Password1` PC name: `Walter-PC`

## Setting up AD, Groups and Policies

* Login to windows server \(domain controller\)
* Server Manager &gt; Dashboard &gt; Tools &gt; Active Directory Users and Computers
* Right click root domain eg `ADHACKING.local` &gt; New &gt; Organizational Unit &gt; Groups
* Move all users \(except Administator and Guest\) from `Users` directory to `Groups` 
* Right click under `Users` directory &gt; create the following users 

For all user creation do:

* \[uncheck\] must change pass at next logon 
* \[check\] password never expires

Copy "administrator" domain admin account logon:`gus` pass:`Password2020@!`

Copy "administrator" domain admin account logon:`SQLService` pass:`MYpassword123#` Description:`Password is MYpassword123#`

New &gt; User domain user account logon:`saul` pass: `Password123`

New &gt; User domain user account logon:`walter` pass: `Password123`

```text
    A no-no here is giving domain-admin rights to service accounts like SQL Service. Even though it is a fake account, in real life situation there are cases where domain-admin rights are given to these kind of services. Also the description where administrators sometimes think its a good idea to put their passwords there thinking no one else can see it.
```

## Configuring File server \(Opening SMB\)

```text
    SMB File sharing is enabled for allowing 445,139 ports to be opened so that we can hack them in lab later.
```

* Server Manager &gt; Dash board &gt; \(side menu\) File and Storage Services &gt; Shares
* TASKS &gt; New Share &gt; SMB Share - Quick &gt; Next
* Share name: `hackme` &gt; Next until &gt; Create

## Creating Service Principal Name \(SPN\)

```text
    Setting up for kerberoasting attack 
```

* Win &gt; Command Prompt &gt; Run as administrator
* Set spn using `setspn -a myDomainController/SQLService.ADHACKING.local:60111 ADHACKING\SQLService`
* Check that spn is set `setspn -T ADHACKING.local -Q */*`

## Group Policy Configuration

```text
    Disabling windows defender for lab purposes (topics like AV bypass and evasion is not covered in this lab)
```

* Win &gt; Group Policy Management &gt; Run as administrator
* Expand Forest &gt; Domains &gt; `ADHACKING.local` &gt; right click &gt; Create a GPO in this domain &gt; `Disable Windows Defender`
* Expand Forest &gt; Domains &gt; `ADHACKING.local` &gt; right click `Disable Windows Defender` &gt; Edit
* Computer Configuration &gt; Policies &gt; Administrative Template &gt; Windows Components &gt; Windows Defender Antivirus
* double click 'Turn off Windows Defender Antivirus' &gt; \[check\] Enabled

## Connecting all machines

```text
    On the both machine, create folder and setup network share. 
    On first machine, give first domain-user a local administrator rights.
    On second machine, give BOTH domain-user local administrator rights.
```

1. Create network share folder
2. Create a folder `share` in C: drive
3. Right click &gt; Properties &gt; Sharing &gt; Share &gt; Share &gt; Yes, turn on network discovery...
4. Configure network settings
5. Open network & Internet settings &gt; change adapter options &gt; IPv4 &gt; Prefered DNS Server 
6. Enter IP address of Domain controller here.
7. Join domain
8. Win &gt; domain &gt; Access work or school &gt; Connect &gt; 'Join this device to local Active Directory domain'
9. Enter domain name `ADHACKING.local`
10. Join domain as `Administrator` pass: `P@$$w0rd`
11. When prompt for add an account &gt; Skip &gt; Restart now
12. Upon restart &gt; select other user and login as domain user eg. `saul` pass: `Password123`
13. Giving local administrator rights
14. Win &gt; Computer Management &gt; Local Users and Groups &gt; Groups &gt; double click Administrators
15. Add &gt; eg. `saul` &gt; Check names &gt; Apply &gt; OK
16. Turn on Network Discovery Computer &gt; Network &gt; Click OK when prompted &gt; top menu bar &gt; Click "Turn on network discovery..."
17. Verify computers join domain
18. On Domain controller &gt; Active Directory Users and Computers &gt; `ADHACKING.local` &gt; Computers
19. See that there are 2 computers added
20. Setting up for mitm6 attack lab example
21. Login to Domain controller &gt; Server Manager &gt; Dashboard &gt; Add roles and features &gt; Next 
22. [x] Active Directory Certificate Services &gt; Add features &gt; Next 
23. [x] Restart the destination server automatically &gt; Install
24. Click Flag notification icon &gt; configure active directory services on the destination server 
25. Credentials: `ADHACKING\Administrator`
26. [x] Certification authority &gt; Next
27. Validity: 99 years &gt; Next &gt; Configure

