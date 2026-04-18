# 🏗️ Lab Architecture
## 📌 Overview
This document outlines the architecture of my cybersecurity home lab, built on a macOS host using virtualization. The lab simulates an attacker-target environment with monitoring and logging capabilities.

## 🖥️ High-Level Architecture
graph TD
    A[Mac Host - macOS] --> B[Hypervisor]

    B --> C[Windows VM (Target)]
    B --> D[Kali Linux VM (Attacker)]

    C <-->|Internal Network| D

## 🔍 Component Breakdown
### 💻 Host Machine

- macOS system running virtualization software
- Provides compute resources for all virtual machines

### 🪟 Windows VM (Target + Monitoring)

Role: Target system for attacks and monitoring

Tools Installed:
- Splunk Enterprise (SIEM)
- Wireshark (Packet Analysis)

Data Collected:
- Windows Event Logs
- Network traffic
### 🐉 Kali Linux VM (Attacker)

Role: Simulated attacker machine

Tools Used:
- Nmap (network scanning)
- Metasploit (exploitation)
- Burp Suite (web testing)

### 🔄 Data Flow
- Kali Linux initiates scans or attacks
- Windows VM receives traffic
- Wireshark captures packet-level data
- Splunk ingests system and security logs
- Logs and traffic are analyzed for suspicious activity

### 🛡️ Security Considerations
- Lab is fully isolated from the internet
- No unauthorized external testing is performed
- Intended strictly for educational purposes
