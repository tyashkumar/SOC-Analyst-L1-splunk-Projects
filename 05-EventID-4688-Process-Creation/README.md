# Event ID 4688 - New Process Creation Detection

## 📌 Description
This use case monitors the execution of new processes, binaries, or command-line arguments on a corporate endpoint to detect post-compromise activity and malicious executions.

---

## 🔍 Splunk Query
```splunk
source="security.evtx" EventCode=4688 
| table _time, host, Account_Name, Process_Name, CommandLine
