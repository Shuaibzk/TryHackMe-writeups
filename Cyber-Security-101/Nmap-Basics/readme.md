```markdown id="nmapbasics01"
# Nmap: The Basics (TryHackMe)

## 🧠 What is Nmap?

Nmap (Network Mapper) is a powerful network scanning and reconnaissance tool used to discover hosts, open ports, running services, and operating systems.

It is widely used for:

- Network discovery
- Security auditing
- Port scanning
- Service enumeration
- Vulnerability assessment preparation

---

## 🔍 Why Nmap Matters

Nmap helps analysts and administrators:

- Identify live hosts on a network
- Discover open or filtered ports
- Detect services and versions
- Estimate operating systems
- Troubleshoot firewall rules
- Understand network exposure

---

## 🌐 Basic Scan Types

## List Scan

nmap -sL TARGET

Lists targets without scanning them.

### Use Case
Useful for verifying target ranges before scanning.

---

## Host Discovery

nmap -sn TARGET

Performs ping scan only (host discovery).

### Use Case
Checks which systems are online without port scanning.

---

## 🚪 Port Scanning Techniques

## TCP Connect Scan

nmap -sT TARGET

Uses full TCP three-way handshake.

### Use Case
Reliable when raw packet privileges are unavailable.

---

## TCP SYN Scan

nmap -sS TARGET

Uses only the first part of the handshake (SYN).

### Use Case
Faster and more stealthy than TCP connect scan.

---

## UDP Scan

nmap -sU TARGET

Scans UDP ports.

### Use Case
Useful for discovering services such as DNS, DHCP, SNMP, NTP.

---

## Fast Scan

nmap -F TARGET

Scans the 100 most common ports.

### Use Case
Quick overview of common exposures.

---

## Port Range Selection

nmap -p 22,80,443 TARGET

Scan selected ports.

nmap -p 1-1000 TARGET

Scan port range.

nmap -p- TARGET

Scan all ports (1–65535).

---

## Treat Host as Online

nmap -Pn TARGET

Skips host discovery and assumes target is online.

### Use Case
Useful when ICMP is blocked by firewall.

---

## 🔬 Service Detection

## OS Detection

nmap -O TARGET

Attempts to identify operating system.

---

## Version Detection

nmap -sV TARGET

Detects running services and versions.

### Example Output
- Apache 2.x
- OpenSSH
- nginx
- MySQL

---

## Aggressive Scan

nmap -A TARGET

Enables:

- OS detection
- Version detection
- Script scanning
- Traceroute (in many cases)

### Use Case
Detailed enumeration scan.

---

## ⏱️ Timing Controls

## Timing Templates

nmap -T0 TARGET

Paranoid (very slow)

nmap -T1 TARGET

Sneaky

nmap -T2 TARGET

Polite

nmap -T3 TARGET

Normal (default)

nmap -T4 TARGET

Aggressive

nmap -T5 TARGET

Insane (very fast)

### Use Case
Control speed vs stealth.

---

## Parallelism

--min-parallelism <num>
--max-parallelism <num>

Controls number of simultaneous probes.

---

## Packet Rate

--min-rate <num>
--max-rate <num>

Controls packets sent per second.

---

## Host Timeout

--host-timeout 30s

Stops scanning a host after timeout.

---

## 📡 Real-Time Output

## Verbose Mode

nmap -v TARGET

nmap -vv TARGET

Shows more progress and details.

---

## Debug Mode

nmap -d TARGET

nmap -d9 TARGET

Displays debugging information.

---

## 📄 Saving Reports

## Normal Output

nmap -oN result.txt TARGET

---

## XML Output

nmap -oX result.xml TARGET

Useful for tools and automation.

---

## Grepable Output

nmap -oG result.grep TARGET

Useful for filtering results quickly.

---

## All Formats

nmap -oA scanname TARGET

Creates:

- scanname.nmap
- scanname.xml
- scanname.gnmap

---

## 🧩 Practical Examples

### Quick Scan of Common Ports

nmap -F 192.168.1.1

---

### Detect Services

nmap -sV 192.168.1.1

---

### Full Port Scan

nmap -p- 192.168.1.1

---

### Aggressive Enumeration

nmap -A 192.168.1.1

---

### Scan Host with Firewall Blocking Ping

nmap -Pn 192.168.1.1

---

## 💡 What I Learned

- Nmap is essential for reconnaissance and enumeration
- Different scan types serve different goals
- TCP, SYN, and UDP scans behave differently
- Service detection reveals useful target information
- Timing options balance stealth and speed
- Saving output improves reporting and workflow

---

## 🛠️ Tool Used

- Nmap
- Terminal / Linux CLI

---

## ⚠️ Note

This writeup focuses on Nmap options, scan types, and learning methodology only.  
Specific challenge answers, flags, and target-specific solutions are intentionally excluded.
```
