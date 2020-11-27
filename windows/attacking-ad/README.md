# Attacking AD



* Microsoft Virtual Academy
* LM,NTLM, Net-NTLMv2 - [https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4](https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4)
* Tim Medin - Attacking Kerberos: Kicking the Guard Dog of Hades
* PayloadAllTheThings AD Attack Cheatsheet - [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology and](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and)

### AD BASICS

**AD DS Data store:**

* Juicy file `Ntds.dit` in `%SystemRoot%\NTDS`

**AD DS Schema**

* Defines type of objects stored in directory. Eg. Class \(User, Computers\), Attribute \(display name, uid\)
* Enforce rules for object creation/config

**Domains**

* Collection of objects eg. computers. Essentially an authentication/authorization boundary

**Trees**

* Hierachy of domains. Eg. Parent, child relationships
* Default: 2-way transitive trust with other domains \(Parent and child\)

**Forests**

* Collection of **Trees**
* Sharing common config, cataloging for searching

**OU Organizational Units**

* Containers for users, groups, computers etc
* Apply policies
* Permission delegation

**Trusts**

* Directional: Trust between domain only. Eg. You and me, we are friends.
* Transitive: Trust between domain extended to include other trusted domains. Eg. You and me, your friends included are my friends.
* All domains in forest trust all other domains in the forest
* Trust can extend outside the forest

**Objects**

* User
* InetOrgPerson: Similar to user account, used for compatibility with other directory services
* Contacts
* Groups
* Computers
* Printers
* Shared folders

