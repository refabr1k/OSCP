# 2020-12 Solarwind supply chain

[https://www.fireeye.com/blog/threat-research/2020/12/evasive-attacker-leverages-solarwinds-supply-chain-compromises-with-sunburst-backdoor.html](https://www.fireeye.com/blog/threat-research/2020/12/evasive-attacker-leverages-solarwinds-supply-chain-compromises-with-sunburst-backdoor.html)

* A Widespread breach that affected many public and private org around the world
* Gain access to victims through Trojanized updates to Solarwind's Orion IT Monitoring and management software

## How?

* An legit .dll component \(digitally signed component of Orion software framework\) has  backdoor
* The backdoor can make HTTP communications to third party servers. 
* The backdoor is capable of performing recon, profiling system, disable system services, transfer, execute files
* Hides network traffic behind a known protocol \(Orion Improvement Protocol\) to blend in with on going activities
* Hides recon results within legitimate configuration files with other Solarwind activities.

## Behavior?

* Uses multiple obfuscated blocklist to identify forensic and antivirus tools running as processes, services and drivers.
* It sits dormant for 2 weeks, before "waking up" to retrieve and execute "jobs" from their C2.
* It resolves to a subdomain of "avsvmcloud.com" after waking up where the DNS response will return a CNAME record that points to a C2 domain.
* The C2 traffic to the malicious domain is designed to mimic normal SolarWinds API commuinications 
* The list of known malicious infrastructure is available on FireEye’s [GitHub page](https://github.com/fireeye/sunburst_countermeasures).

