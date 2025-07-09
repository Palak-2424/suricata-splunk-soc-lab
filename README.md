# Suricata + Splunk SOC Lab

This project showcases how to build a lightweight, virtual SOC (Security Operations Center) by integrating **Suricata IDS** with **Splunk** for real-time alerting, log analysis, and attack simulation using Kali Linux.

---

## 🧰 Tools Used

- **Suricata** – Intrusion Detection System (IDS)
- **Splunk Enterprise** – Log management and dashboarding
- **Kali Linux** – Attacker machine
- **Ubuntu VM** – Hosts Suricata and Splunk
- **Nmap, curl** – Simulated attacks

---

## 🛠️ Lab Setup

### 1. Environment

- **Virtualization**: UTM (macOS)
- **VMs**:
  - Ubuntu (Suricata + Splunk)
  - Kali Linux (Attacker)

### 2. Suricata Installation

```bash
sudo apt update
sudo apt install suricata
