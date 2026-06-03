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
 
### 🔴 Case 1: Malicious Outbound Traffic
* **Destination IP:** `185.220.100.254`
* **Network Port:** `443` (HTTPS) / `80` (HTTP)
* **Attack Type:** Anonymized Traffic / Potential Command & Control (C2) Communication
  
* **OSINT Verification:** <img width="907" height="479" alt="image" src="https://github.com/user-attachments/assets/0b31b4ba-c121-4398-99d2-63a21009cb58" />

### 📋 SOC Analyst Verdict: True Positive (TP)
* **Reasoning:** OSINT enrichment via threat intelligence platforms confirms that the destination IP is a verified Tor Exit Node. Corporate policy strictly prohibits unauthorized anonymized traffic. 
* **Severity:** High / Critical (Due to potential data exfiltration risk).
  
**Since it is a True Positive**
  ### 🛡️ Incident Response & Mitigation Actions

1. **Host Isolation (Containment):** Immediately isolate the source endpoint from the corporate network using the EDR tool (e.g., CrowdStrike, Defender) to block further communication or potential data exfiltration.
2. **Network Blocking:** Coordinate with the Network/Firewall team to block outbound traffic to the malicious IP across the entire organizational boundary.
3. **Log Investigation (Splunk Hunting):** * Review process execution logs (**Event ID 4688** / Sysmon) around the time of the alert to identify which process/binary initiated the connection.
   * Run a Splunk query to calculate total `bytes_out` to determine if any massive data exfiltration took place.
4. **Credential Remediation:** Force a password reset for the user account associated with the compromised endpoint, treating the credentials as potentially compromised.
