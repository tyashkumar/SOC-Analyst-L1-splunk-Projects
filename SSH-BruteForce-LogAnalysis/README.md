# Hands- On Lab: SSH Brute-Force Attack Simulation & Log Analysis

## Project Overview 
This Project Explain about Brute-force attack simulation on kali linux.Real-security logs of failed login attempts and what soc anayalst will take action.

# Tools used
-Operating system- Oracle kali Linux
-Attack Tool: Hydra(Atool used to guess passwords quickly)
-Defensive Tool: journalctl(The Built-in tool to view system logs in newer versions of kali)
-Framework:MITRE ATT&CK matrix(A guide used to identify hacker techniques)

**Step-by-Step Execution & Methodology**

## The command used 
## Step 1
```bash
sudo systemctl status ssh
```
## Output
<img width="841" height="302" alt="image" src="https://github.com/user-attachments/assets/af652611-0e62-48f8-a823-c14a9fc826a4" />

**First, I turned on the SSH service on my machine. Then, I used Hydra to launch a password-guessing attack against my own local account (kali) using a built-in password list file.**
## step 2 
hydra -l kali -P /usr/share/wordlists/fasttrack.txt ssh://127.0.0.1 -t 4
<img width="869" height="255" alt="image" src="https://github.com/user-attachments/assets/238f9c95-06a7-4189-ab6b-1dcb78f33c55" />

## step 3
**Finding the Logs**
**sudo journalctl _SYSTEMD_UNIT=ssh.service | grep "Failed password"**

Newer verison of Kali Linux do not use the old text file at /var/log/auth.log. Instead, logs are saved in a system database. To pull up the failed password attempts, I used this command to filter the database

## Threat Analysis & Evidence
## Threat Analysis & Evidence
```text
Jun 05 08:38:48 kali sshd-session[17611]: Failed password for kali from 127.0.0.1 port 46526 ssh2
Jun 05 08:38:48 kali sshd-session[17610]: Failed password for kali from 127.0.0.1 port 46516 ssh2
```
## Explanation-
**-Jun 05 08:38:48 -> When it happened**
**-kali -> The computer name: The name of your virtual machine (the hostname)**
**-sshd-session[17611] -> The program tracking it: The SSH service on your computer handled this connection request. The number in brackets is just an ID the system gave to this specific connection try.**
**-Failed password for kali -> What happened: Someone tried to log into the account named kali but typed the wrong password.**
**-from 127.0.0.1 -> Where it came from:"The Exact computer"**
**port 46526 -> The doorway used: The random tracking port Hydra used to send that specific password guess**
**-ssh2 -> The connection type-used  verison 2 of the secure shell protocol**

## SOC Action Looking at this task 

1. **Detect:** Catches the sudden spike of automated `Failed password` alerts on the SIEM dashboard.
2. **Investigate:** Uses `journalctl` to spot inhuman speeds (multiple attempts in the same second) to confirm it is a hacking tool.
3. **Classify:** Maps the attack to the MITRE ATT&CK Matrix as **[Brute Force: Password Guessing (T1110.001)](https://attack.mitre.org/techniques/T1110/001/)**.
4. **Ticket & Route:** Logs as True Postive high-priority incident ticket and routes it to the specific Tier 2 or Windows/Network Security team for escalation.
5. **Respond:** Blocks the attacking IP address at the firewall and locks the target account to stop the breach.
