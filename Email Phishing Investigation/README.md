# Email Phishing Investigation – IPL Reward Spam Campaign

# Overview 
**This investigation was conducted on a suspicious email received with the subject:"IPL final ke liye reward"**
The main movtive for **Investigation -rekhamanchu@its1winin.com** is to **identify Email is Legitimate or part of a phsishing spam.**

# Email Overview
<img width="1210" height="576" alt="spam Email 1" src="https://github.com/user-attachments/assets/aea1439c-47c3-43aa-9521-a55a06dbb7a9" />

**Email Details**
**Subject - 🏏 IPL final ke liye reward**
**Sender -rekhamanchu@its1winin.com**
**Category - Spam**
**Delivery Date- 01 june 2026**

**Observation**
The  Email claimed that a reward/free bet was available for the IPL Final and contained URL Link encouraging the receipient to clcik and  claim the offer.

**Investigation Process**
**1.Email Header Analysis -Using MXToolbox Email Header Analyzer.**
<img width="1483" height="823" alt="spam Email" src="https://github.com/user-attachments/assets/4cdd2f99-e8da-4a91-9dac-421802895c50" />

**Findings**
**- SPF Authentication: Pass
- SPF Alignment: Pass
- DKIM Authentication: Pass
- DKIM Alignment: Pass
- DMARC: No Record Found**

 SPF and DKIM validation succeeded,But the absence of a DMARC policy reduces protection against domain spoofing.

**2.Source IP Identification**
The header analysis identified the originating **IP address:34.234.88.95**
**Google relay IP observed:209.85.220.65**
The Google IP belongs to Gmail infrastructure and was excluded from threat attribution.

**3. IP Reputation Analysis**
The originating IP was analyzed using VirusTotal.

**Findings**
IP Address: 34.234.88.95
ASN: Amazon.com, Inc. (AS14618)
**Detection Ratio: 0/91**
<img width="912" height="529" alt="image" src="https://github.com/user-attachments/assets/75031e07-843a-418f-ac01-cab568a309d1" />
**No security vendors flagged the IP as malicious at the time of analysis**

****4. URL Investigation-URL found within the email:https://1wn.la/7q5k**
<img width="696" height="556" alt="image" src="https://github.com/user-attachments/assets/9133faee-debf-4a86-b846-2f47d8321474" />

**The URL was analyzed using URLScan.io -https://1wn.la/7q5k**

**Redirect Chain**
1.1wn.la
2.one-vv0990.com
3.bundlecda.com
4.Domain created on:June 03, 2026 (Recently registered domain,Redirecting users to an online bettering  platform)
The link redirected through multiple domains before reaching the final destination.

**Observation**
The email was identified as a suspicious spam email. It used an **IPL reward offer** to encourage users to click a link that redirected to a **betting-related**website. Although the email passed SPF and DKIM checks, the overall behavior of the message was suspicious and could expose users to scams or unwanted websites.

**SOC Analyst Action**
**-Status: Escalated and Blocked**
**Reason:**
Suspicious reward-based URL,Redirects to external betting website.
Classified as spam by Gmail.**Potential risk to users**

**Recommended Actions**
Block the identified URLs and domains.(Using tools like Office 365, Proofpoint etc)
Add the **sender email/domain to monitoring or block lists**.
Escalate the case to the Security Team for review.
Inform users not to interact with the links.

**Final Status: Escalated & Blocked**
