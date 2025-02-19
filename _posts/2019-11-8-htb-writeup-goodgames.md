---
layout: single
title: Sau - Hack The Box
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
GoodGames is an easy Linux machine that demonstrates the significance of sanitizing user inputs to prevent SQL injection attacks. It also highlights the importance of using strong hashing algorithms to protect database passwords from being cracked, and the risks of password reuse. Additionally, it underscores the dangers of using the render_template_string function in Python web applications, which can lead to Server Side Template Injection (SSTI) vulnerabilities. The privilege escalation process involves Docker host enumeration and illustrates how gaining admin privileges in a container, while being a low-privileged user on the host machine, can lead to serious security issues, allowing attackers to escalate privileges to compromise the entire system.


## Skills Required

- Web Enumeration
- Basic Web Exploitation Skills
- Basic Hash Cracking Skills
- Understanding of SSTI Detection & Exploitation
- Understanding of Basic Docker Security Concepts
- Skills Learned

- Exploiting Union-Based SQL Injections
- Cracking Weak Hash Algorithms
- Risks of Password Reuse
- Exploiting SSTI
- Basics of Docker Breakouts
- Enumeration

## Nmap Scan
We begin by running a quick Nmap scan on port 80, which is hosting a Python 3.9.2 application. The website appears to be gaming-themed, with the title "GoodGames" and a footer indicating it's hosted on goodgames.htb. We add the domain to our hosts file and examine the login feature.

## SQL Injection Test
A simple SQL injection test (admin' or 1=1 -- -) indicates that a valid email address is required to bypass the login. By capturing the login request with BurpSuite and modifying the email to inject our payload, we successfully authenticate as an admin user.

## SQLMap Enumeration
After confirming the SQL injection vulnerability, we use SQLMap to enumerate the database and extract sensitive information. The database and table names are identified, revealing a hashed admin password. Using CrackStation, we crack the hash to gain further access.

## SSTI Vulnerability

Navigating to the user details settings, we notice a potential SSTI vulnerability in the username field of the Python Flask application. Injecting the payload {{7*7}} results in the username being changed to 49, confirming that the application is vulnerable to SSTI.

By base64 encoding a reverse shell payload and injecting it through the username field, we gain a shell on the server.

## Privilege Escalation via Docker Escape

Upon obtaining a shell on the system, we realize we're inside a Docker container. Further investigation reveals that the home directory of the augustus user from the host machine is mounted within the container with read/write permissions. This allows us to modify files on the host machine from within the container.

## Docker Host Enumeration
We attempt a port scan of the Docker host (IP: 172.19.0.1) and find that SSH is open. By attempting password reuse, we successfully log in as the augustus user on the host.

## Escalating to Root
From within the container, we copy the Bash binary into the mounted user directory and change its ownership to root:root, applying SUID permissions. After exiting the container, we SSH back into the augustus user on the host and confirm that the new Bash binary has the necessary permissions to escalate to root.

Finally, we execute the Bash binary with SUID permissions, effectively gaining root privileges on the host system.