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
