---
layout: single
title: Phishing URL Detected | Phishing Email Analysis
excerpt: "A detailed walkthrough of analyzing and responding to a phishing URL detection alert in a SOC environment from Let's Defend."
date: 2024-10-13
classes: wide
header:
  teaser: /blog/assets/images/phishing-alert/phishing-detection.png
  teaser_home_page: true
  icon: /blog/assets/images/cybersecurity-alert.webp
categories:
  - Incident Response
---

# SOC141 — Phishing URL Detected**  

## **Triggered Alert**  
On **March 22nd**, an alert was triggered, indicating a **phishing URL** detected on the host system **EmilyComp** (IP: **172.16.17.49**). We will now claim the ticket and proceed with the investigation following our playbook.  
![](../assets/images/phishing-url-detected/1.png)

![](../assets/images/phishing-url-detected/2.png)

![](../assets/images/phishing-url-detected/3.png)

---

## **Playbook Investigation**  
Our investigation follows standard procedures, focusing on critical details:  

### **1. Source and Destination IP Analysis**  
- The **source IP (172.16.17.49)** is linked to **EmilyComp**.  
- The **destination IP (91.189.114.8)** requires further analysis using **VirusTotal** and **AbuseIPDB** to determine its reputation.  
![](../assets/images/phishing-url-detected/4.png)

![](../assets/images/phishing-url-detected/5.png)


### **2. Threat Intelligence Reports**  
- **VirusTotal**: One security vendor has flagged the destination IP as **malicious**.  
- **AbuseIPDB**: The IP originates from **Russia**, raising further concerns.  
![](../assets/images/phishing-url-detected/6.png)

- Next, we investigate the **requested URL** for malicious activity.  
---

## **Malicious URL Confirmation**  
Reports confirm that the detected URL is indeed **malicious** and associated with **phishing campaigns**. This reinforces the need for deeper log analysis.  
![](../assets/images/phishing-url-detected/7.png)

---

## **Log Analysis**  
Reviewing network logs, we identify **two instances** where the **malicious IP and URL** were accessed. Both requests originated from **EmilyComp (172.16.17.49)**, confirming that her device is the **only affected host**.  

![](../assets/images/phishing-url-detected/8.png)

![](../assets/images/phishing-url-detected/9.png)

---

## **Mitigation Actions**  
To prevent further risk:  
1. **Isolate the affected host** via the **Endpoint Security** system.  
![](../assets/images/phishing-url-detected/10.png)

2. **Document findings** in the **Analyst Note** section.  
![](../assets/images/phishing-url-detected/11.png)

3. **Preserve artifacts** for further analysis.  
![](../assets/images/phishing-url-detected/12.png)

4. **Close the alert and update the playbook.**  
![](../assets/images/phishing-url-detected/13.png)

---

## **Phishing Prevention Best Practices**  
To reduce the risk of phishing attacks, individuals and organizations should:  

✅ **Enable Multi-Factor Authentication (MFA):** Adds an extra layer of security.  
✅ **Verify Links & Senders:** Double-check email addresses and URLs before clicking.  
✅ **Use Strong, Unique Passwords:** Avoid reuse and consider a password manager.  
✅ **Keep Software Updated:** Patches vulnerabilities that attackers exploit.  
✅ **Be Wary of Urgent Requests:** Avoid acting hastily on suspicious emails or messages.  

By following these precautions, we can strengthen defenses against phishing threats.  
