# Event ID 4625 - Failed Login Detection

## Description
This is case detects failed  login attempts in Windows Security logs using splunk.

## Splunk Query
**source="security.evtx" EventCode=4625 | table _time, host, Account_Name, Logon_Type, IpAddress, Failure_Reason
**
<img width="1375" height="373" alt="4625 failed login" src="https://github.com/user-attachments/assets/98d08a2e-7a1f-48c6-b2ff-3bfdee2247d8" />
## SOC Use Case
This event is used to detect:
- Brute force attacks
- Password guessing attempts
- Unauthorized login attempts
- Suspicious authentication behavior

## Login Types
- 2-> Interactive login attempt  /Normal login
- 3-> Network Login attempt
- 10-> Remote Desktop login attempt
  
---

## SOC Investigation Flow and  neccessary steps
If multiple 4625 events are followed by 4624:
-> Possible brute force success attack
Example:
4625 -> 4625-> 4625 ->4624
If suspicious/malicious:

block IP
disable user account
raise incident ticket
start investigation
check endpoint logs
After seeing 4625 logs, SOC analyzes patterns, correlates with other events (like 4624), identifies brute force attempts, checks source IP/user behavior, and decides whether it is normal activity or a security incident.
---

##  Fields Explained
- _time → Event timestamp
- host → System name (anonymized)
- Account_Name → Username attempted
- Logon_Type → Type of login attempt
- IpAddress → Source IP address
- Failure_Reason → Reason for failure

---


## 🔐 Data Privacy
All data used is lab-generated and fully anonymized for educational purposes.

---

## 👨‍💻 Author
SOC Analyst L1 Practice Project (Splunk Lab)
