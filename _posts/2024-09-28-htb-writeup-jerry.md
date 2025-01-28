---
layout: single
title: Jerry - Hack The Box
excerpt: ""
date: 2024-10-04
classes: wide
header:
  teaser: /blog/assets/images/htb-writeup-writeup/
  teaser_home_page: true
  icon: /blog/assets/images/hackthebox.webp
categories:
  - WriteUp
#tags:
 # - linux
  #- http
---

#### **SYNOPSIS**  
Although Jerry is considered an easier machine on Hack The Box, it provides a realistic challenge, as Apache Tomcat is frequently exposed and often configured with default or weak credentials.

#### **Skills Required**  
- Basic knowledge of Python, Ruby, or familiarity with web brute-force attack tools.

#### **Skills Learned**  
- Basic script debugging.  
- Creation of custom WAR file payloads.  
- SILENTTRINITY post-exploitation framework installation and usage (following the IppSec Jerry video).

### **Enumeration**

**Nmap Scan**

To identify open ports, I used masscan for an initial scan:  

masscan -p1-65535 10.10.10.95 --rate=1000 -e tun0 > ports

Next, I extracted the open ports and ran a more thorough Nmap scan to identify services:

ports=$(cat ports | awk -F " " '{print $4}' | awk -F "/" '{print $1}' | sort -n | tr '\n' ',' | sed 's/,$//')
nmap -Pn -sV -sC -p$ports 10.10.10.95


This revealed that Apache Tomcat was running on the default port, and the `/manager/html` page was accessible.


### **Identification of Valid Credentials**

I investigated if it was worth attempting default or common credentials. Daniel Miessler’s SecLists provides a comprehensive list of default Tomcat credentials, which can be found [here](https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt).

The list contains 79 credentials, so I used a brute-force attack to automate the process, leveraging the `tomcat-brute.py` script. This revealed that the username `tomcat` with the password `s3cret` was valid.


### **Exploitation**

**Creating a WAR File**

To exploit the system, I used the `make-war.sh` script to create a WAR file. The WAR file included the "jsp File Browser 1.2" by Boris von Loesch, which allows file system enumeration and executing system commands.

The source code for the JSP browser can be found [here](https://raw.githubusercontent.com/tennc/webshell/master/jsp/jspbrowser/Browser.jsp).

**Deploying the Webshell**

After logging into the Web Manager and deploying the WAR file via the "WAR file to deploy" section, the "wshell" application was successfully uploaded. From here, I could interact with the system and proceed with further exploitation.


Let me know if you need any further adjustments or additional details!