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
source="security.evtx" EventCode=4798 
| stats count by Process_Name, Account_Name, ComputerName
| sort - count
<img width="1788" height="872" alt="Screenshot 2026-06-02 130201" src="https://github.com/user-attachments/assets/83d7c011-8b4c-4471-8703-bf366054f015" />
