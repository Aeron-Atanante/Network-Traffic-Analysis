# Home Lab: Network Traffic Analysis & Threat Detection

## Overview
This project focuses on capturing, analyzing, and detecting threats in network traffic using Wireshark. It includes simulations of brute-force attacks and port scanning to identify potential security threats.

## Prerequisites
- **Host Machine:** Ubuntu
- **Virtual Machines:** Kali Linux (attacker), Windows 10 (victim)
- **Tools:** Wireshark, Nmap, Hydra

## 1. Capturing Network Traffic
1. **Open Wireshark** on Ubuntu.
2. **Select the Network Interface**:
   - Use `ip a` or `ifconfig` to find the active network interface.
   - Choose the appropriate interface in Wireshark (e.g., `eth0`, `wlan0`).
3. **Start Packet Capture**:
   - Click the **Start** button.
   - Generate network traffic between VMs.
   - Stop capture after a few minutes.
4. **Save the Capture File**:
   - Go to `File > Save As`.
   - Name it `network_traffic.pcap`.

![Wireshark Capture](PLACEHOLDER-Wireshark-Capture.png)

## 2. Simulating Threats & Analyzing Traffic

### A. Brute-Force Attack Simulation
1. **On Kali Linux, run Hydra for brute-force login attempts:**
   ```bash
   hydra -l admin -P rockyou.txt ssh://[Target IP]
   ```
2. **On Wireshark, filter SSH traffic:**
   ```
   ssh
   ```
3. **Analyze Results:**
   - Identify repeated login attempts.
   - Observe response codes (failed/successful logins).

![Brute Force Capture](PLACEHOLDER-Brute-Force.png)

### B. Port Scanning Simulation
1. **On Kali Linux, scan the Windows 10 machine using Nmap:**
   ```bash
   nmap -sS -p- [Target IP]
   ```
2. **On Wireshark, filter SYN packets:**
   ```
   tcp.flags.syn == 1 && tcp.flags.ack == 0
   ```
3. **Analyze Results:**
   - Identify open ports.
   - Detect unusual scanning patterns.

![Port Scan Capture](PLACEHOLDER-Port-Scan.png)

## 3. Threat Detection Insights
- **Indicators of Compromise (IoCs):**
  - Multiple failed SSH login attempts (brute-force attack).
  - Rapid connection attempts to multiple ports (port scanning).
- **Defensive Measures:**
  - Implement SSH rate limiting.
  - Use firewalls to block scanning attempts.

## 4. Conclusion
This experiment showcases how to detect network threats using Wireshark. It emphasizes proactive threat detection and security monitoring.

## Screenshots
Replace `PLACEHOLDER` images with actual screenshots from your experiments.

---
**Author:** Aeron Atanante  
**Portfolio Project for Cybersecurity**
