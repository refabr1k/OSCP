# T:88 - Kerberos

#### ENUM KEBEROS USERS

```bash
nmap -sV --script krb5-enum-users --script-args krb5-enum-users.realm='domain.local',userdb='/usr/share/wordlist/userlist.txt' <target ip> -p88


```



