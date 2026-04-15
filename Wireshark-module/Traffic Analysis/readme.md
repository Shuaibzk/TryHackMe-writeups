room link: https://tryhackme.com/room/wiresharktrafficanalysis
level: medium

# ARP Spoofing and Flooding
finding arp request created by attacker-> 
using sender's ip attribute from packet details and applying it as a filter


# Identifying Hosts: DHCP, NetBIOS, and Kerberos (TryHackMe)

## 🧠 Objective
Learn how to identify hosts and users in network traffic using DHCP, NetBIOS, and Kerberos protocols in Wireshark.

---

## 🔍 DHCP Analysis

### Goal
Identify hosts and requested IP addresses using DHCP traffic.

### Method
- Filter DHCP request packets:
  dhcp.option.dhcp == 3

- To find a specific device by hostname:
  dhcp.option.hostname contains "<device_name>"

### Key Idea
DHCP packets reveal:
- Hostnames
- Requested IP addresses
- Network activity of devices

---

## 🌐 NetBIOS Analysis

### Goal
Identify workstation registration requests.

### Method
- Look for NetBIOS Name Service (NBNS) queries
- Filter by hostname:
  nbns.name contains "<workstation_name>"

### Key Idea
NetBIOS helps map:
- Hostnames to IP addresses
- Workstation presence in the network

---

## 🔐 Kerberos Analysis

### Goal
Identify users and hostnames from authentication traffic.

### Method

#### 🔹 Finding a specific user:
- kerberos.CNameString contains "<username>"
- Exclude machine accounts:
  !(kerberos.CNameString contains "$")

#### 🔹 Identifying hostnames:
- kerberos.CNameString contains "$"

### Key Idea
- User accounts → no `$`
- Machine accounts → end with `$`

---

## 💡 What I Learned
- DHCP can be used to track device identity and IP requests
- NetBIOS helps identify workstation names in a network
- Kerberos traffic reveals both users and machine accounts
- Filtering is essential for efficient packet analysis

---

## ⚠️ Note
This writeup focuses on learning concepts and methodology.  
Sensitive details such as actual answers, flags, or target-specific data are intentionally omitted.



# Tunnelling Traffic: ICMP and DNS (TryHackMe)

## 🧠 Objective
Learn how attackers may abuse ICMP and DNS protocols to tunnel traffic, hide communication, or transfer data through trusted network protocols.

---

# 🔍 ICMP Tunnelling Analysis

ICMP is normally used for diagnostics (such as ping), but attackers may misuse it to carry hidden traffic.

## Suspicious ICMP Filter

wireshark
(data.len > 64) and (icmp contains "ssh" or icmp contains "ftp" or icmp contains "tcp" or icmp contains "http")


# Cleartext Protocol Analysis: FTP

--> Request = client
--> Response = server

response has one key attribute: Code
request has two attribute: Command & Argument(arg)
  - Commands are like: USER, PASS, CWD, LIST, MKD, RMD, MTDM, SITE, SIZE
  
## Overview
FTP is used to transfer files but standard FTP does not encrypt usernames, passwords, or file data. This makes it useful to analyze in Wireshark and risky in insecure networks.

## Risks
- Credential theft
- Unauthorized access
- MITM attacks
- Malware upload/download
- Data exfiltration

## Basic Filter
ftp

## FTP Response Codes

### x1x Information
- 211 = System status
- 212 = Directory status
- 213 = File status

Filter:
ftp.response.code == 211

### x2x Connection
- 220 = Service ready
- 227 = Entering passive mode
- 228 = Long passive mode
- 229 = Extended passive mode

Filter:
ftp.response.code == 227

### x3x Authentication
- 230 = Login success
- 231 = Logout
- 331 = Username valid, need password
- 430 = Invalid username/password
- 530 = Not logged in / invalid password

Filter:
ftp.response.code == 230

## FTP Commands

Username:
ftp.request.command == "USER"

Password:
ftp.request.command == "PASS"

Specific password:
ftp.request.arg == "password"

Other commands:
- CWD = Change directory
- LIST = List files

## Threat Hunting

Failed logins:
ftp.response.code == 530

Failed logins for target username:
(ftp.response.code == 530) and (ftp.response.arg contains "username")

Password spray:
(ftp.request.command == "PASS") and (ftp.request.arg == "password")

## What I Learned
- FTP sends data in cleartext
- USER and PASS may expose credentials
- Response codes reveal login behavior
- Repeated failures may indicate brute-force attempts

## Recommendation
Use secure alternatives:
- SFTP
- FTPS
- SCP