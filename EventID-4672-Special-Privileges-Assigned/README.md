# Event ID 4672 - Privilege Escalation Detection

## Description
This  use case detects when a user is assigned special privileges during login.

----
##splunk Query
**source="security.evtx" EventCode=4672
| table _time, host, Account_Name, Privileges**
<img width="1402" height="856" alt="4672" src="https://github.com/user-attachments/assets/c58cd79d-8590-480c-a48a-630df5626eab" />
## SOC Use Case
This event is used to detect:
-Admin-level logins
-Privilege escalation attempts
-Unauthorized high -privilege access

---
## Soc Importance 
Event 4672 is critical because it indicates:
-Elevated access granted 
- possible attacker gaining admin rights
- - post -compromise behavior
  - 
## SOC Action and steps taken by them 
- Verify user legitimacy
- check login source IP
- Correalte with 4625/4624
- check process  logs(4688)
- raise HIGH severity alert if suspicious
- escalate to L2 and open incident ticket
- start investigation
  
