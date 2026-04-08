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
