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
