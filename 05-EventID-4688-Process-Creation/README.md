# Event ID 4688 - New Process Creation Detection

## 📌 Description
This use case monitors the execution of new processes, binaries, or command-line arguments on a corporate endpoint to detect post-compromise activity and malicious executions.

---

## 🔍 Splunk Query

```spl
source="security.evtx" EventCode=4688 
| table _time, host, Account_Name, Process_Name, CommandLine

<img width="1318" height="988" alt="4688" src="https://github.com/user-attachments/assets/4f0d7911-342a-48eb-a9ce-55504b0b262f" />



**SOC Use Case Analysis
-SOC analysts monitor Event ID 4688 to map application execution baselines:

Suspicious Binaries: Identifying untrusted programs launching from temporary directories.

Living off the Land: Catching administrative tools being abused (e.g., cmd.exe /c whoami).

## Steps & Actions Taken by a SOC Analyst (Incident Triage)
-**Analyze the Command Context**:Check the commandLine field field. Is the binary running a standard corporate task, or is it trying to gather system data (e.g., whoami, net user, ipconfig /all)?
-**Verify Path Legitimacy:** Ensure standard windows processes are running from their correct paths (e.g., svchost.exe should only launch from C:\Windows\System32\, never from a Temp folder).

**Check Account Permissions**: Determine if the process was launched by a standard domain user or escalated to high privileges like SYSTEM or LocalAdmin.
**Determine Escalation Actions (L1 to L2):**
If False Positive: If the process belongs to a known internal IT maintenance script or software update, document the baseline and close the ticket.

If Suspicious/Malicious: If an unapproved binary executed malicious command-line arguments, preserve the event timeline data and immediately escalate the incident ticket to the Level 2 (L2) Response Team for full endpoint containment.
