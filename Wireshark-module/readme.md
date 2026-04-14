# Wireshark 101

- TryHackMe room: <https://tryhackme.com/room/wireshark>
- OS: `N/A`

This document is  a full writeup: The learn material + question + answers + bonus of the amazing Wireshark room.

Have added some stuff, but still all credits goes to the [TryHackMe](https://tryhackme.com/) community.

##  Introduction

Wireshark, a tool used for creating and analyzing PCAPs (network packet capture files), is commonly used as one of the best packet analysis tools. In this room, we will look at the basics of installing Wireshark and using it to perform basic packet analysis and take a deep look at each common networking protocol.

![alt text](C:/Users/Shuaib/Pictures/wireshark_logo.png "Wireshark logo")

PCAPs used in this room have been sourced from the Wireshark Sample Captures Page as well as captures from various members of the TryHackMe community. All credit goes to the respective owners.

Before completing this room we recommend completing the '[Introductory Networking](https://tryhackme.com/room/introtonetworking)'. If you have a general knowledge of networking basics then you will be ready to begin.

##  Installation

Wireshark is available on Windows, macOS, and Linux.

On Linux, it can often be installed with a package manager such as:

```bash
sudo apt install wireshark
```

The installation for Wireshark is very easy and typically comes with a packaged GUI wizard. Luckily if you're using Kali Linux (or the TryHackMe AttackBox) then it is already installed on your machine. Wireshark can run on Windows, macOS, and Linux. To begin installing Wireshark on a Windows or macOS device you will need to first grab an installer from the [Wireshark website](https://www.wireshark.org/download.html). Once you have downloaded an installer, simply run it and follow the GUI wizard.

If you are using Linux you can install Wireshark with `apt-get install wireshark` or a similar package manager.

![alt text](C:/Users/Shuaib/Pictures/wireshark_shark.png "Shark Logo")

Note: Wireshark can come with other packages and tools; you can decide whether or not you want to install them along with Wireshark.

For more information about Wireshark check out the [Wireshark Documentation](https://www.wireshark.org/docs/).

## Wireshark Interface Basics

When Wireshark opens, it shows available network interfaces and their activity. From there, you can either start a live capture or open an existing PCAP file for analysis.

Important columns in the packet list usually include:

- Packet number
- Time
- Source
- Destination
- Protocol
- Length
- Info

## Filtering

Filtering is one of the most important Wireshark skills, especially when working with large captures.

### Useful Basic Filters

```wireshark
ip.addr == 10.10.10.10
tcp.port == 80
udp.port == 53
dns
```

## ARP Analysis

ARP maps IP addresses to MAC addresses inside a local network.

## ICMP Analysis

ICMP is commonly used for diagnostics and testing.

Example suspicious ICMP filter:

```wireshark
(data.len > 64) and (icmp contains "ssh" or icmp contains "ftp" or icmp contains "tcp" or icmp contains "http")
```

## DNS Analysis

Useful filters:

```wireshark
dns.qry.name.len > 40 and !mdns
dns.qry.name.len > 40 and !mdns && dns.qry.name contains ".com"
```

## HTTP / HTTPS

HTTP is readable plaintext traffic. HTTPS is encrypted traffic and requires keys for decryption in supported lab environments.

## What I Learned

- Filtering saves time
- Protocol behavior tells a story
- DNS and ICMP can be abused for tunneling
- Packet analysis is about context

## Note

This repository contains personal study notes and methodology only. Room-specific answers, flags, and direct solution values are intentionally omitted.
