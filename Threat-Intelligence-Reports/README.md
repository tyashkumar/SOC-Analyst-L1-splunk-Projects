# Threat Intelligence Reports

## Overview
This section focuses on Integrating Threat Intelligence (TI) feeds and Indicators of Compromise (IOCs) into our SOC workflows to proactively hunt for threats and accelerate incident response.

## Core Concepts & Tools
* **Threat Intel Feeds:** (e.g., AlienVault OTX, MISP, Abuse.ch)
* **Frameworks:** MITRE ATT&CK / Cyber Kill Chain
* **Formats:** STIX / TAXII

## Active Projects / Use Cases
1. **Malicious IP Correlation:** Cross-referencing Splunk authentication logs with active threat feeds.
2. **Phishing IOCs:** Tracking known malicious domains and hashes.
3. 
### 🔴 Case 1: Lecture Lab Verification - Tor Exit Node / Malicious Outbound Traffic
* **Destination IP:** `185.220.100.254`
* **Network Port:** `443` (HTTPS) / `80` (HTTP)
* **Attack Type:** Anonymized Traffic / Potential Command & Control (C2) Communication
* **Analyst Breakdown:** This specific IP address serves as a verified Tor Exit Node. In a corporate SOC environment, outbound traffic destined for a Tor exit node triggers a high-severity alert. Threat actors leverage Tor infrastructure to obfuscate their true identity during data exfiltration or active scanning phases.
* **OSINT Verification:** ![VirusTotal Screenshot](./artifacts/vt-case1.png)

---

### 📊 Practical Triage Simulation (Based on Coursework)
Following the exact incident workflow demonstrated in the **Day 18 SOC Course**, here is how the triage plan maps out:

1. **Detection:** SIEM logs capture an active outbound connection from internal host `10.0.0.15` to `185.220.100.254`.
2. **OSINT Verification:** Analyst pivots the IP to VirusTotal/AbuseIPDB and confirms its malicious reputation as a public Tor proxy.
3. **Scope Analysis:** Analyst searches internal firewall and DNS logs to uncover the total blast radius (discovering if other internal hosts have contacted it).
4. **Incident Response Trigger:** The confirmed IOC shifts into "Evidence of Compromise," initiating host isolation protocols via EDR.
