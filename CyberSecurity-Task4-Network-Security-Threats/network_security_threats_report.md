Common Network Security Threats
Task: Oasis Infobyte SIP – Cyber Security Task 4  
File: `network\_security\_threats\_report.md`  
Prepared: August 2026
---
1. Introduction
Modern organizations depend on interconnected networks for cloud applications, remote access, online transactions, communication, and critical business operations. This connectivity also creates opportunities for attackers to disrupt services, intercept communications, impersonate trusted systems, or redirect users to malicious destinations. Network security threats are therefore not limited to data theft: they can affect confidentiality, integrity, and availability, as well as business continuity, customer trust, and reputation. NIST Cybersecurity Framework (CSF) 2.0 emphasizes managing cybersecurity risk as an organizational risk and provides a structured approach for identifying, protecting against, detecting, responding to, and recovering from cyber events. The threats discussed in this report—DoS/DDoS, Man-in-the-Middle (MITM), IP spoofing, and DNS poisoning/spoofing—illustrate four different ways attackers can undermine network trust and availability.
---
2. DoS and DDoS Attacks
2.1 What is a DoS/DDoS attack?
A Denial-of-Service (DoS) attack attempts to make a system, service, application, or network unavailable or significantly less responsive to legitimate users. The attacker achieves this by exhausting resources such as network bandwidth, CPU, memory, connection tables, or application capacity.
A Distributed Denial-of-Service (DDoS) attack uses many sources simultaneously. These sources may be compromised computers, IoT devices, cloud systems, or other infrastructure controlled or abused by an attacker.
MITRE ATT&CK classifies Network Denial of Service as T1498, with sub-techniques for direct network flooding (T1498.001) and reflection amplification (T1498.002). Direct flooding sends a large volume of traffic toward the target. Reflection/amplification attacks abuse third-party services: the attacker sends requests containing a spoofed victim IP address, causing reflectors to send responses to the victim. DNS, NTP, and exposed Memcached services have historically been abused for this purpose.
2.2 How the attack works
A simplified DDoS process is:
Preparation: The attacker obtains access to many traffic-generating systems or identifies vulnerable amplification services.
Target selection: A website, DNS service, API, server, or network is selected.
Traffic generation: The attacker causes large numbers of requests or packets to be sent toward the target.
Resource exhaustion: Bandwidth, connection state, CPU, memory, or application resources become saturated.
Service degradation: Legitimate users experience slow responses, errors, or complete unavailability.
Mitigation: Traffic is detected, filtered, rate-limited, or diverted to a DDoS mitigation provider.
2.3 Real-world example: GitHub Memcached DDoS, 2018
On February 28, 2018, GitHub was hit by a very large DDoS attack that peaked at approximately 1.35 Tbps. The attack used Memcached reflection/amplification, rather than a traditional malware-controlled botnet. GitHub experienced intermittent disruption, but its automated response redirected traffic through Akamai Prolexic, where malicious traffic was filtered. The incident lasted roughly 20 minutes and demonstrated how exposed UDP services could be abused to produce enormous traffic volumes.
Source: WIRED, GitHub Survived the Biggest DDoS Attack Ever Recorded.
2.4 Impact
Website and application downtime.
Loss of revenue and productivity.
Increased bandwidth and infrastructure costs.
Reduced availability of DNS, email, APIs, or cloud services.
Customer dissatisfaction and reputational damage.
Potential distraction of security teams while another attack occurs.
In reflection attacks, third-party infrastructure may unintentionally participate in the attack.
2.5 Three specific mitigation strategies
1. Use upstream DDoS protection/CDN traffic scrubbing
Route internet-facing services through a DDoS mitigation provider or CDN capable of absorbing large traffic volumes and filtering malicious packets before they reach the organization. This is especially important when attack traffic exceeds the capacity of the organization's own internet connection.
2. Apply rate limiting, filtering, and network controls
Use firewalls, routers, load balancers, WAFs, and upstream filtering to restrict unnecessary protocols, ports, and abnormal traffic rates. For UDP amplification, stateful inspection and appropriate rate limiting can reduce exposure.
3. Maintain a tested DDoS response and business-continuity plan
Define detection thresholds, escalation contacts, ISP/provider contacts, traffic-diversion procedures, and recovery priorities before an attack occurs. Regularly test the response process so that mitigation does not depend on improvised decisions during an outage.
---
3. Man-in-the-Middle (MITM) Attacks
3.1 What is a MITM attack?
A Man-in-the-Middle (MITM) attack occurs when an attacker positions themselves between two communicating parties and causes traffic to pass through attacker-controlled infrastructure. The attacker may then observe, capture, relay, or modify communications.
MITRE ATT&CK uses the broader term Adversary-in-the-Middle (AiTM) and identifies it as T1557. Sub-techniques include ARP cache poisoning, name-resolution poisoning, DHCP spoofing, and Evil Twin attacks.
3.2 How the attack works
A simplified MITM process is:
Positioning: The attacker obtains a privileged network position or creates a malicious network path.
Redirection: Traffic is made to flow through the attacker-controlled system.
Interception: The attacker observes or relays network communications.
Credential/data capture: If traffic is unencrypted or authentication is weak, sensitive information may be exposed.
Manipulation: In some cases, the attacker modifies requests or responses before forwarding them.
Persistence or follow-on activity: Captured credentials, cookies, or tokens may be used for unauthorized access.
Common positioning methods include ARP poisoning on a local network, rogue/Evil Twin Wi-Fi access points, DNS manipulation, and exploitation of weaknesses in TLS/certificate validation.
3.3 Real-world example: DigiNotar, 2011
In 2011, attackers compromised the Dutch certificate authority DigiNotar and obtained fraudulent certificates, including one for Google. The fraudulent certificate was used in an active MITM attack against Google services. The incident demonstrated the importance of certificate authorities in the Internet's trust model: if a trusted CA is compromised, an attacker may be able to impersonate a legitimate service to victims.
The incident caused browsers to distrust DigiNotar's certificates, and the company ultimately collapsed.
Sources: Mozilla Security Advisory and WIRED's report on the DigiNotar compromise.
3.4 Impact
Theft of usernames, passwords, session cookies, and tokens.
Exposure of confidential communications.
Modification of data in transit.
Redirection to malicious services.
Credential theft and account takeover.
Loss of trust in network infrastructure or certificate authorities.
3.5 Three specific mitigation strategies
1. Enforce strong TLS/HTTPS and certificate validation
Use current TLS configurations, validate certificates and hostnames correctly, and prevent applications from accepting invalid or unexpected certificates. NIST's TLS guidance emphasizes proper certificate validation and trusted certificate management.
2. Secure the local network
Use WPA2/WPA3 enterprise security where appropriate, network segmentation, switch security, DHCP snooping, ARP inspection, and wireless intrusion detection/prevention. Disable unnecessary legacy protocols such as LLMNR/NBT-NS where operationally possible.
3. Monitor for network and configuration anomalies
Monitor DNS, ARP, DHCP, routing, certificate, and authentication events. Unexpected ARP mappings, rogue DHCP responses, certificate changes, or abnormal DNS behavior can indicate an attempt to establish an adversary-in-the-middle position.
---
4. IP Spoofing
4.1 What is IP spoofing?
IP spoofing is the manipulation of the source IP address in a network packet so that it appears to originate from another system or network. The packet's true sender is therefore different from the source address presented to the recipient.
IP spoofing is commonly used as an enabling technique rather than a complete attack by itself. It can support reflection/amplification DDoS attacks, bypass poorly designed source-IP trust rules, or make attribution more difficult.
4.2 How the attack works
A simplified process is:
The attacker creates a packet.
The attacker replaces the legitimate source IP address with a forged address.
The packet is transmitted toward the destination.
The receiving system sees the forged source address.
Depending on the protocol and attack type, the recipient may respond to the spoofed address.
In reflection attacks, the spoofed address belongs to the victim, causing third-party systems to send their responses to the victim.
IP spoofing is particularly effective with protocols that do not establish a reliable state before accepting traffic, especially UDP-based reflection scenarios.
4.3 Real-world example: spoofing in the 2018 GitHub DDoS
The 2018 GitHub Memcached DDoS provides a clear real-world example of IP spoofing as an attack-enabling technique. The attackers sent requests to exposed Memcached servers while using the victim's IP address as the spoofed source address. The Memcached servers then returned amplified responses to GitHub.
This demonstrates why source-address validation matters: without filtering that prevents packets from leaving a network with forged source addresses, an attacker can abuse unrelated systems as reflectors.
4.4 Impact
Enables reflection/amplification DDoS attacks.
Makes source attribution more difficult.
Can undermine security controls that incorrectly trust source IP addresses.
Can generate fraudulent traffic appearing to originate from trusted networks.
May cause collateral traffic to systems whose addresses were spoofed.
4.5 Three specific mitigation strategies
1. Implement ingress and egress filtering (BCP 38/BCP 84)
Network providers should prevent packets with impossible or unauthorized source addresses from entering or leaving their networks. CISA specifically recommends ingress filtering to block spoofed packets, referencing BCP 38 and BCP 84.
2. Do not use source IP address as the only authentication mechanism
Access controls should use stronger identity and authentication mechanisms rather than assuming that a packet is trustworthy merely because its source IP looks familiar. Combine network controls with cryptographic authentication, identity-aware access controls, and authorization.
3. Use stateful firewalls and anti-spoofing controls
Deploy firewalls and routers configured to reject traffic with invalid source addresses, unexpected internal addresses on external interfaces, or otherwise impossible routing characteristics. Unicast Reverse Path Forwarding (uRPF), where appropriate, can help reject spoofed traffic.
---
5. DNS Poisoning / DNS Spoofing
5.1 What is DNS poisoning?
The Domain Name System (DNS) translates human-readable domain names such as `example.com` into IP addresses. DNS poisoning or spoofing occurs when an attacker causes a resolver, client, or authoritative DNS infrastructure to use a fraudulent DNS response or record.
A successful attack can make users believe they are connecting to a legitimate service while actually being redirected to an attacker-controlled IP address.
DNS poisoning can occur at different levels:
Cache poisoning: False DNS information is inserted into a resolver's cache.
DNS hijacking: An attacker compromises DNS management, authoritative servers, or registrar infrastructure and changes legitimate DNS records.
Local DNS manipulation: Malware or network attacks alter the resolver configuration or hosts file.
DNS response spoofing: An attacker attempts to provide a forged response that is accepted as legitimate.
5.2 How the attack works
A user requests the IP address of a legitimate domain.
The DNS resolution process is manipulated.
A forged or unauthorized DNS record/response provides an attacker-controlled IP address.
The user is redirected to the malicious destination.
The attacker may steal credentials, distribute malware, intercept traffic, or display fraudulent content.
If the attacker controls authoritative DNS infrastructure, the redirection can affect many users.
5.3 Real-world example: Sea Turtle DNS hijacking campaign
The Sea Turtle campaign, documented by Cisco Talos in 2019, targeted organizations and DNS-related infrastructure in the Middle East and North Africa. Cisco reported that at least 40 organizations across 13 countries were compromised.
The attackers compromised DNS infrastructure and modified name-server records so that users could be redirected to attacker-controlled servers. In some cases, the attackers established MITM servers that captured credentials before forwarding users to legitimate services.
This incident demonstrates that DNS attacks can affect not only individual computers but also the infrastructure responsible for directing large groups of users.
5.4 Impact
Users can be redirected to malicious websites.
Credentials and session information can be stolen.
Malware can be delivered through redirected traffic.
Email traffic can potentially be redirected.
Organizations can lose control over public-facing domain infrastructure.
DNS manipulation can undermine trust in the Internet's naming system.
5.5 Three specific mitigation strategies
1. Deploy DNSSEC where supported
DNSSEC adds cryptographic authentication to DNS data, helping resolvers detect forged DNS responses and unauthorized modifications to signed zones. NIST identifies DNSSEC as a protection against DNS cache-poisoning attacks.
2. Strongly secure registrar and DNS-management accounts
Use MFA, strong unique credentials, least privilege, registrar locks where available, and tightly controlled administrative access. CISA recommends reviewing DNS records, changing DNS-management passwords, enabling MFA, and monitoring certificate-transparency logs for unauthorized certificates.
3. Continuously monitor DNS records and certificates
Maintain an inventory of organizational domains and authoritative DNS servers. Monitor for unexpected changes to NS, A, AAAA, MX, and other important records. Certificate Transparency monitoring can reveal unauthorized certificates issued for organizational domains.
---
6. Comparison of Threats
Threat	Primary Attack Vector	Who Is at Risk?	Difficulty to Execute	Ease of Mitigation
DoS/DDoS	High-volume traffic, botnets, or reflection/amplification	Websites, APIs, DNS, cloud services, enterprises	Medium	Medium
MITM	ARP/DNS/DHCP manipulation, rogue Wi-Fi, weak TLS validation	Users on exposed/local networks; organizations handling sensitive traffic	Medium–High	Medium–High
IP Spoofing	Forged source IP addresses in network packets	Networks, DDoS victims, services relying on source IP trust	Medium	Medium
DNS Poisoning/Spoofing	Forged DNS responses or compromised DNS/registrar infrastructure	Internet users, domains, enterprises, DNS providers	Medium–High	Medium–High
Interpretation: Difficulty depends heavily on the attacker's position, available infrastructure, target configuration, and security controls. Mitigation is also contextual. For example, an organization cannot completely prevent a large volumetric DDoS attack from being generated, but it can greatly reduce its impact through upstream filtering and resilient architecture.
---
7. Recommended Defensive Architecture
A network administrator should treat these threats as interconnected rather than isolated.
A practical defense-in-depth approach is:
```text
                    Internet
                       |
              +------------------+
              | DDoS / CDN Layer |
              +------------------+
                       |
              +------------------+
              | Firewall / WAF   |
              +------------------+
                       |
              +------------------+
              | Network IDS/IPS  |
              +------------------+
                       |
          +------------+------------+
          |                         |
   +-------------+           +-------------+
   | Public Zone |           | Internal    |
   | / DMZ       |           | Network     |
   +-------------+           +-------------+
                                      |
                          +-----------+-----------+
                          |                       |
                    DNS / Identity          User Devices
                          |
                    DNSSEC + MFA
                    + Monitoring
```
Key principles:
Availability: DDoS protection, redundancy, capacity planning, rate limiting, and tested recovery procedures.
Confidentiality: TLS, secure Wi-Fi, network segmentation, and strong authentication.
Integrity: DNSSEC, certificate validation, secure routing, and configuration monitoring.
Visibility: Centralized logs, IDS/IPS, DNS monitoring, network telemetry, and alerting.
Identity: MFA and least privilege for DNS, network, cloud, and administrative accounts.
---
8. Conclusion: Three Key Takeaways for a Network Administrator
1. Build defense in depth
No single firewall, IDS, DNS control, or encryption mechanism can stop every network attack. Layered controls reduce the chance that one weakness becomes a major incident.
2. Protect the network's trust mechanisms
DNS records, certificates, routing information, DHCP, ARP, and source addresses are part of the infrastructure that users implicitly trust. These mechanisms must be protected, monitored, and authenticated wherever possible.
3. Prepare for incidents before they occur
A network administrator should maintain monitoring, documented escalation procedures, tested backups/recovery plans, upstream-provider contacts, and clear response playbooks. NIST's incident-response guidance emphasizes preparation, detection and analysis, response, and lessons learned as important parts of effective incident handling.
---
9. References
NIST — Cybersecurity Framework (CSF) 2.0. National Institute of Standards and Technology, 2024.  
https://www.nist.gov/publications/nist-cybersecurity-framework-csf-20
NIST SP 800-52 Rev. 2 — Guidelines for the Selection, Configuration, and Use of Transport Layer Security (TLS) Implementations.  
https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-52r2.pdf
NIST SP 800-61 Rev. 1 — Computer Security Incident Handling Guide.  
https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-61r1.pdf
NIST SP 800-81-2 — Secure Domain Name System (DNS) Deployment Guide.  
https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-81-2.pdf
CISA — UDP-Based Amplification Attacks.  
https://www.cisa.gov/ncas/alerts/ta14-017a
CISA — Mitigate DNS Infrastructure Tampering.  
https://www.cisa.gov/sites/default/files/publications/CISAInsights-Cyber-MitigateDNSInfrastructureTampering_S508C.pdf
MITRE ATT&CK — Network Denial of Service (T1498).  
https://attack.mitre.org/techniques/T1498/
MITRE ATT&CK — Adversary-in-the-Middle (T1557).  
https://attack.mitre.org/techniques/T1557/
SANS Institute — Denial of Service Deterrence.  
https://www.sans.org/white-papers/35877
SANS Institute — Address Resolution Protocol Spoofing and Man-in-the-Middle Attacks.  
https://www.sans.org/white-papers/474
WIRED — GitHub Survived the Biggest DDoS Attack Ever Recorded.  
https://www.wired.com/story/github-ddos-memcached/
WIRED — DigiNotar Bankruptcy Following Certificate Compromise.  
https://www.wired.com/2011/09/diginotar-bankruptcy/
Cisco Talos — DNS Hijacking Abuses Trust in Core Internet Service (Sea Turtle).  
https://blog.talosintelligence.com/seaturtle/
Mozilla Security Advisory — Protection against fraudulent DigiNotar certificates.  
https://www.mozilla.org/en-US/security/advisories/mfsa2011-34/
---
10. Source-to-Requirement Mapping
Task Requirement	Covered In
Introduction	Section 1
DoS/DDoS explanation	Section 2.1–2.2
DoS/DDoS real-world example	Section 2.3 — GitHub 2018
DoS/DDoS impact	Section 2.4
3 DoS/DDoS mitigations	Section 2.5
MITM explanation	Section 3.1–3.2
MITM real-world example	Section 3.3 — DigiNotar
MITM impact	Section 3.4
3 MITM mitigations	Section 3.5
IP Spoofing explanation	Section 4.1–4.2
IP Spoofing real-world example	Section 4.3 — GitHub Memcached attack
IP Spoofing impact	Section 4.4
3 IP Spoofing mitigations	Section 4.5
DNS Poisoning/Spoofing explanation	Section 5.1–5.2
DNS real-world example	Section 5.3 — Sea Turtle
DNS impact	Section 5.4
3 DNS mitigations	Section 5.5
Comparison table	Section 6
Network administrator takeaways	Section 8
4+ credible references	Section 9
Markdown format	Entire report

