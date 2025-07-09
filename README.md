# Suricata + Splunk SOC Home Lab

This project demonstrates how to build a lightweight **Security Operations Center (SOC)** lab using **Suricata IDS**, **Splunk**, and **Kali Linux** for cyberattack detection, alerting, and visualization. It's ideal for SOC analyst and cybersecurity learners to simulate, detect, and visualize attacks like Nmap scans, test rule triggers, and real-time traffic analysis.

---

## Lab Setup

- **Suricata IDS** v7.0.10 (installed on Ubuntu 22.04)
- **Splunk Enterprise (free license)** for log visualization and dashboards
- **Kali Linux VM** to simulate attacks (e.g., Nmap scans)
- **UTM** (macOS virtualization platform)
- **eve.json log format** for Suricata-Splunk integration

---

## System Architecture

```

\[Kali Linux VM] ➜ Simulates attacks (Nmap)
│
▼
\[Suricata on Ubuntu] ➜ Detects and logs alerts to eve.json
│
▼
\[Splunk on Ubuntu] ➜ Ingests logs and displays dashboards

````

---

## Suricata Installation (Official Method)

Ref: [https://docs.suricata.io/en/latest/quickstart.html](https://docs.suricata.io/en/latest/quickstart.html)

```bash
sudo apt update
sudo apt install -y software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install -y suricata
````

---

## 🚀 Post-Installation Steps

### Update Rules

```bash
sudo suricata-update
```

### Test Configuration

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -v
```

### Start Suricata

```bash
sudo systemctl start suricata
```

---

## Simulating Alerts

### Test Rule Trigger

Trigger built-in test rule with:

```bash
curl http://testmynids.org/uid/index.html
```

Check Suricata alert log:

```bash
sudo tail -f /var/log/suricata/fast.log
```

### Add Custom Nmap Rule

Create rule file:

```bash
sudo nano /var/lib/suricata/rules/nmap-test.rules
```

Paste rule:

```
alert tcp any any -> any any (msg:"[TEST] Nmap SYN Scan Detected"; flags:S; threshold:type both, track by_src, count 10, seconds 10; sid:9999991; rev:1;)
```

Edit `/etc/suricata/suricata.yaml` to include:

```yaml
rule-files:
  - suricata.rules
  - nmap-test.rules
```

Restart Suricata:

```bash
sudo systemctl restart suricata
```

###  Run Nmap Scan from Kali

```bash
nmap -sS 10.0.0.181
```

---

## 📊 Splunk Integration

### Start Splunk

```bash
sudo /opt/splunk/bin/splunk start
```

Access Splunk at: [http://localhost:8000](http://localhost:8000)

### Add Suricata Logs

* Go to **Settings > Add Data > Monitor File**
* File path: `/var/log/suricata/eve.json`
* Source type: `suricata`
* Index: `main` (or custom if created)

---

## 📈 Splunk Dashboard Panels

### 🔍 Panel 1: Recent Alerts Table

```spl
index=* source="/var/log/suricata/eve.json"
event_type=alert
| table _time, src_ip, src_port, dest_ip, dest_port, proto, alert.signature
| sort -_time
```

### 📊 Panel 2: Top 10 Alert Signatures (Bar Chart)

```spl
index=* source="/var/log/suricata/eve.json"
event_type=alert
| top limit=10 alert.signature
```

---

## 🔐 Observations

* Suricata successfully detected known and custom attacks
* eve.json format enabled structured alert data ingestion
* Splunk displayed dynamic dashboards with real-time events
* SOC-style workflow simulated from detection to visualization

---

## 📁 Project Structure

```
suricata-splunk-soc-lab/
├── README.md
├── suricata/
│   └── rules/
│       └── nmap-test.rules
├── logs/
│   └── eve.json (optional copy)
```

---

## 👩‍💻 Author

**Palak G.**
Cybersecurity Student & SOC Enthusiast
Linux | Splunk | IDS/IPS | Python
Built as a personal learning project to simulate SOC workflows.

---

## 📝 License

This project is licensed under the **MIT License**. Free to use, modify, and share.

---

## 📚 References

* [Suricata Official Docs](https://docs.suricata.io/en/latest/)
* [Splunk Docs](https://docs.splunk.com/)
* [Emerging Threats Rules](https://rules.emergingthreats.net/)

```

---

✅ This is ready to paste into a GitHub repo as `README.md`. Let me know if you'd like a `gitignore` or help pushing to GitHub.
```
