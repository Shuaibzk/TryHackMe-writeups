# Tcpdump: The Basics (TryHackMe)

## 🧠 What is Tcpdump?

Tcpdump is a command-line packet analyzer used to capture and inspect network traffic in real time or from saved packet capture files.

It is widely used for:

- Network troubleshooting
- Traffic monitoring
- Security investigations
- Protocol analysis

---

## 🔍 Why Tcpdump Matters

Tcpdump helps analysts:

- Capture packets from network interfaces
- Read `.pcap` files offline
- Filter traffic efficiently
- Detect suspicious connections
- Investigate protocols such as ARP, DNS, TCP, UDP, and ICMP

---

## ⚙️ Basic Commands

### Capture on an Interface

tcpdump -i eth0

Captures packets on a specific interface.

---

### Capture on All Interfaces

tcpdump -i any

Listens on all available interfaces.

---

### Write Capture to File

tcpdump -w capture.pcap

Stores captured traffic in a `.pcap` file for later analysis.

---

### Read from File

tcpdump -r capture.pcap

Reads packets from a saved capture file.

---

### Capture Specific Number of Packets

tcpdump -c 50

Stops after capturing 50 packets.

---

## 🧩 Useful Options

tcpdump -n  
Do not resolve IP addresses.

tcpdump -nn  
Do not resolve IP addresses or protocol numbers.

tcpdump -v  
Verbose output.

tcpdump -vv  
More verbose.

tcpdump -vvv  
Maximum verbosity.

tcpdump -q  
Quick / brief output.

tcpdump -e  
Include MAC addresses.

tcpdump -A  
Display packet contents in ASCII.

tcpdump -xx  
Display packet contents in hexadecimal.

tcpdump -X  
Display packet contents in hexadecimal and ASCII.

---

## 🔗 Logical Operators in Filters

Tcpdump supports:

- and
- or
- not

These help combine conditions for precise filtering.

---

## 🌐 Traffic Filtering Examples

### SSH Traffic on All Interfaces

tcpdump -i any tcp port 22

Captures TCP traffic to or from port 22 (SSH).

---

### NTP Traffic on WiFi Interface

tcpdump -i wlo1 udp port 123

Captures UDP traffic on port 123 (NTP).

---

### HTTPS Traffic to a Specific Host

tcpdump -i eth0 host example.com and tcp port 443 -w https.pcap

Captures HTTPS traffic related to a specific host.

---

### Capture 50 Verbose Packets

tcpdump -i eth0 -c 50 -v

Captures 50 packets with verbose details.

---

### Capture and Save Until Interrupted

tcpdump -i wlo1 -w data.pcap

Runs until manually stopped with `CTRL + C`.

---

### Capture Without Name Resolution

tcpdump -i any -nn

Displays raw IP addresses and protocol numbers.

---

## 🛡️ Protocol Investigation Examples

## ARP Analysis

### Identify ARP Requests

tcpdump -r traffic.pcap arp -n -e

Useful for viewing ARP traffic and MAC addresses.

### Find Devices Asking for a MAC Address

tcpdump -r traffic.pcap arp host TARGET_IP

Helps identify hosts resolving MAC addresses.

---

## DNS Analysis

### View First DNS Query

tcpdump -r traffic.pcap -c 1 port 53

DNS commonly uses port 53, so filtering by port is useful.

---

## TCP Flags Analysis

### Only SYN Packets

tcpdump "tcp[tcpflags] == tcp-syn"

Packets with only SYN flag set.

---

### SYN Included

tcpdump "tcp[tcpflags] & tcp-syn != 0"

Packets where SYN is present.

---

### SYN or ACK Included

tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"

Packets containing SYN or ACK.

---

### Only Reset (RST) Packets

tcpdump -r traffic.pcap "tcp[tcpflags] == tcp-rst"

Useful when investigating connection failures or resets.

---

## 📦 Packet Size Filtering

### Packets Larger Than a Specific Size

tcpdump -r traffic.pcap greater 15000 -n

Useful for spotting unusual large transfers.

---

## 💡 What I Learned

- Tcpdump is powerful for live traffic capture and offline analysis
- Filters make investigations faster and more precise
- ARP can reveal device relationships on the LAN
- DNS traffic can reveal requested domains
- TCP flags help analyze connection behavior
- Large packets may indicate transfers worth investigating

---

## 🛠️ Tool Used

- Tcpdump
- Terminal / Linux CLI

---

## ⚠️ Note

This writeup focuses on commands, protocol understanding, and filtering methods only.  
Specific challenge answers, flags, and target-specific solutions are intentionally excluded.