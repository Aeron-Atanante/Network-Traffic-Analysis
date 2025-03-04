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
   - Choose the appropriate interface in Wireshark (e.g., `eth0`, `wlan0`, `enp0s3`).
3. **Start Packet Capture**:
   - Click the **Start** button.
   - Generate network traffic between VMs.
   - Stop capture after a few minutes.
4. **Save the Capture File**:
   - Go to `File > Save As`.
   - Name it `network_traffic.pcap`.
![{1BBC2107-1854-4944-9114-A04719A8A503}](https://github.com/user-attachments/assets/ed5cfbdb-e99e-4c58-8571-f01559c50e00)
*Reference Image: Wireshark Capture*

## 2. Simulating Threats & Analyzing Traffic

### A. Brute-Force Attack Simulation
1. **On Kali Linux, run Hydra for brute-force login attempts:**
   ```bash
   hydra -l admin -P rockyou.txt -t 4 192.168.216.6 ssh -v
   ```
2. **On Wireshark, filter SSH traffic:**
   ```
   ssh
   ```
3. **Analyze Results:**
   - Identify repeated login attempts.
   - Observe response codes (failed/successful logins).
![{B26081B7-851D-497D-82CE-B55C9C416A6D}](https://github.com/user-attachments/assets/81cf5f06-96f7-4e93-9fac-0c7cbfdd6dc2)
*Reference Image: Brute Force Capture*

### B. Port Scanning Simulation
1. **On Kali Linux, scan the Windows 10 machine using Nmap:**
   ```bash
   nmap -sS -p- 192.168.216.5
   ```
2. **On Wireshark, filter SYN packets:**
   ```
   tcp.flags.syn == 1 && tcp.flags.ack == 0
   ```
3. **Analyze Results:**
   - Identify open ports.
      ## Identified Open Ports  
   
      | Port       | State | Service        | Description |
      |------------|-------|---------------|-------------|
      | 135        | Open  | msrpc         | Microsoft RPC |
      | 139        | Open  | netbios-ssn   | NetBIOS Session Service |
      | 445        | Open  | microsoft-ds  | SMB over TCP |
      | 5040       | Open  | Unknown       | Possibly related to Windows services |
      | 7680       | Open  | pando-pub     | Likely Windows Update Delivery Optimization |
      | 49664-49670 | Open | Unknown       | Dynamic RPC or other Windows-related services |
   
      These ports suggest that the scanned device is likely a Windows machine, possibly running file-sharing and remote procedure call (RPC) services. The unknown ports may require further investigation.

   - Detect unusual scanning patterns.
      ## 🔍 Indicators of Unusual Scanning Patterns

      ### 1️⃣ Multiple SYN Packets to Different Ports
      - This behavior indicates **port scanning**, a common technique used for reconnaissance.
      - Targeting multiple critical ports suggests an attempt to identify open services.
      
      ### 2️⃣ Rapid Sequence of Requests
      - The requests were sent **almost simultaneously**, which is **not typical** for legitimate traffic.
      - This pattern aligns with automated scanning tools like **Nmap or Masscan**.
      
      ### 3️⃣ Targeting of Critical Services
      - Commonly exploited ports detected:  
        - `22` (SSH), `23` (Telnet), `25` (SMTP), `80` (HTTP), `110` (POP3), `139` (NetBIOS), `3306` (MySQL)
      - Suggests an attacker probing for vulnerabilities.
      
      ## 🚀 Conclusion
      This traffic strongly suggests a **port scan** rather than normal communication. The source IP (`192.168.216.6`) is likely using an automated tool to **enumerate open ports** on `192.168.216.5`.
![{26F11E96-55FD-4ED1-8A52-719CE7F58CD7}](https://github.com/user-attachments/assets/fb6dbcbe-7a08-4a9a-a2b9-9f00ca1179ab)
*Reference Image: Port Scan Capture*

## 3. Threat Detection Insights
- **Indicators of Compromise (IoCs):**
  - Multiple failed SSH login attempts (brute-force attack).
  - Rapid connection attempts to multiple ports (port scanning).
- **Defensive Measures:**
  - Implement SSH rate limiting.
  - Use firewalls to block scanning attempts.

## 4. Conclusion
This experiment showcases how to detect network threats using Wireshark. It emphasizes proactive threat detection and security monitoring.

