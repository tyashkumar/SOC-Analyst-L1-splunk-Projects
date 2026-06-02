# Event ID 4624 - Successful Login Detection

## Description
This use case detects successful authentication events in Windows Security logs.

## Splunk Query
source="security.evtx" EventCode=4624
| table _time, host, Logon_Type, Account_Name, IpAddress

## SOC Use Case
- Detect successful logins
- Monitor RDP access (Logon_Type 10)
- Identify unusual authentication patterns

## Fields
- _time → timestamp
- host → system name
- Logon_Type → login type
- Account_Name → user
- IpAddress → source IP

## Note
All data is lab-generated and anonymized.
<img width="1486" height="339" alt="4624" src="https://github.com/user-attachments/assets/1396dbd6-8edc-470c-8155-81096b6ec5a4" />

**What action and steps will be taken by SOC Analyst**
*- who logged in?(Account_Name)
- From where?(IpAddress)
- How?(Logon_Type)
- When?(time pattern)

 **Analyze Logon Type**

-**2**	Local login	Normal user activity
-**3**	Network login	File/share access check
-**10**	RDP login	⚠️ high interest (remote access)
-**5**	Service login	Ignore (system)

 **soc checks For anomalies**
 ❗ Questions:
Is this login at unusual time?
Is IP internal or external?
Is user normally logging from this host?
Too many logins in short time?
user1 logged in (4624)
host = workstation-01
ip = 10.0.0.5
logon_type = 2 **Normal Login**

**But if they see:**
user1 login from unknown IP
logon_type = 10 (RDP)
multiple attempts

**SOC Conclusion**
Possible:

brute force success
unauthorized access
lateral movement
