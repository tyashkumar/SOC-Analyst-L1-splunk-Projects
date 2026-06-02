#Event ID 4798 -Group Membership Enumeration Detection

## Overview
This project demonstrates detection of windows Security Event ID **4798**, which is generate when a user's local group membership is enumerated.This activity can be used by attackers during privilege escalation and reconnaissance.

## Objective
Detect suspicious enumeration of local group membership using splunk logs.

## Event Details
-**Event ID:** 4798
-**Log Source:** Windows Security Event Logs
-**Category:** Account Management
-**Description:** A user's local group memebrship was enumerated.

## why this Matters
Attackers often check group memebrship to:
-Identify admin users
-Map provilege structure
-Plan lateral movement

## Detection Logic (splunk Query)
**source="security.evtx" EventCode=4798 
| stats count by Process_Name, Account_Name, ComputerName
| sort - count**
<img width="1788" height="872" alt="Screenshot 2026-06-02 130201" src="https://github.com/user-attachments/assets/83d7c011-8b4c-4471-8703-bf366054f015" />
## "How SOC Analyst Investigates This and take Futher step"?
**Analyze the Process & Establish a Baseline:**
*Looking at the top-performing processes.In our dataset, 'svchost.exe'3858,explorer.exe 344'are highly frequence.A SOC analysy recongnize these as normal Windows background operations verifying them out to reduce false- postive.
**Identify Anomalies (The Red Flags):**
* when we look closely at the low-count processes or unexpected tools.If administrative binares like 'cmd.exe','powershell.exe', 'net.exe', or 'whoami.exe' show up executing groupenumeration, it immeditaly indicates active hands-on-keyboard reconnaissance.
* *Pay attention to untrusted binaries executing out of temporary paths (e.g., '\AppData\Local\Temp\').
  
**Take Further steps (The pivot & Escalation):**
* **Scope the Account:** Check if the `Account_Name` performing the enumeration belongs to a **Domain Admin** or a **standard user**. A standard user running these commands is **highly suspicious**.
* **Correlate Timelines:** Copy the specific `ComputerName` or `Account_Name` and run a follow-up query to check a 30-minute window before and after the event. Look for Event ID 4624 (Successful Logons) to find where they connected from, or Event ID 4688 (Process Creation) to see what malicious files they executed next. ***(Look at the next project/section to check how we run and verify Event ID 4624)***.
* **Containment:** If the activity is confirmed malicious, escalate to L2/Incident Response to isolate the workstation from the network.
