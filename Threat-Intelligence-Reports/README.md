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

---

### 🟡 Case 2: Vidar Info-Stealer Command & Control (C2) Traffic
* **Destination IP:** `116.203.3.198`
* **Network Port:** `443` (HTTPS)
* **Malware Family / Attack Type:** Vidar Info-Stealer / Malicious Web-Blending Traffic
* **Why it matters:** Vidar is a highly dangerous malware designed to harvest user credentials, stored browser cookies, session tokens, and cryptocurrency wallets. Because the malware communicates over Port 443, the outbound exfiltration traffic blends into standard encrypted web browsing, hiding the active data theft from basic network firewalls.
* **OSINT Verification:** Checked via ThreatFox and VirusTotal; the IP is verified as an active C2 (Command and Control) infrastructure node hosting Vidar stealer configurations.
  <img width="890" height="476" alt="image" src="https://github.com/user-attachments/assets/ccb5ac41-b0f2-4678-80e5-068be79eca15" />


### 📋 SOC Analyst Verdict: True Positive (TP)
* **Reasoning:** Network proxy and firewall logs confirm an internal corporate endpoint successfully initiated an outbound HTTPS connection to this known malicious C2 IP address. Given the nature of Vidar, this indicates a high probability of endpoint compromise and credential exfiltration.
* **Severity:** Critical
  

### 🛡️ Incident Response & Mitigation Actions
1. **Immediate Host Isolation:** Instantly isolate the affected host machine from the network using the EDR platform to prevent the threat actor from utilizing any stolen session cookies or moving laterally.
2. **Credential Revocation & Force Logout:** Immediately expire all active sessions, invalidate tokens, and force a global password reset for all user accounts that were logged into that specific endpoint.
3. **EDR Triage & Memory Scan:** Run a specialized full-disk and active memory scan via the EDR agent to locate, dump, and terminate the malicious Vidar payload/binary running on the host.
4. **Firewall Blocking:** Add `116.203.3.198` to the perimeter firewall's outbound blocklist organization-wide to disrupt any further communication attempts from other internal assets.

---

### 🔵 Case 3: Vidar Infrastructure Redundancy (Multi-Vector Malware Node)
* **Destination IP:** `135.181.224.72`
* **Network Port:** `443` (HTTPS)
* **Malware Family / Attack Type:** Vidar Infrastructure / Adversary Persistence
* **Why it matters:** This serves as a secondary, distinct infrastructure node hardcoded into the same Vidar malware campaign. Attackers deliberately configure multiple backup C2 IPs. If a SOC analyst detects and blocks the primary IP (like Case 2), the malware running on the compromised endpoint automatically falls back to this active backup node to maintain communication and ensure continuous data exfiltration.
* **OSINT Verification:** Checked via ThreatFox; confirmed as an active multi-vector configuration node tied directly to the same Vidar campaign fingerprint.
* <img width="928" height="488" alt="image" src="https://github.com/user-attachments/assets/d961e2e6-0af1-456c-8c1c-69714292a994" />


### 📋 SOC Analyst Verdict: True Positive (TP)
* **Reasoning:** Cross-correlation of network logs shows traffic attempts to this secondary IP alongside the primary infection alert. This confirms an automated fallback or multi-channel connection sequence from the compromised internal asset.
* **Severity:** Critical

### 🛡️ Incident Response & Mitigation Actions
1. **Proactive Domain/IP Blacklisting:** Immediately block `135.181.224.72` across all corporate perimeter firewalls proactively, ensuring the malware cannot establish its fallback connection channel.
2. **Scoping and Scope Expansion:** Run a Splunk search across the entire environment to see if *any other* endpoints are attempting to connect to this backup IP, which would reveal hidden infections.
3. **Advanced Threat Hunting:** Analyze the malware configuration file (if extracted by the EDR tool) to identify if there are any further obfuscated IP addresses or backup domains in the pool.
