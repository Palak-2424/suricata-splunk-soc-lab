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
+------------------+       +----------------------+       +----------------------+
|  Kali Linux VM   | ----> | Suricata on Ubuntu   | ----> |   Splunk Dashboard   |
|  (Attack Source) |       | (Network Detection)  |       |  (Log Visualization) |
+------------------+       +----------------------+       +----------------------+

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
## 📚 References

* [Suricata Official Docs](https://docs.suricata.io/en/latest/)
* [Splunk Docs](https://docs.splunk.com/)
* [Emerging Threats Rules](https://rules.emergingthreats.net/)

Here are clean, GitHub-optimized Markdown headers for the content you shared — you can **copy-paste directly into your `README.md`**:

---

## 🚀 Next Steps & Future Enhancements

* Integrate **ELK Stack** for advanced log correlation and analytics
* Automate simulated attacks using **Python scripts or shell**
* Expand detection capabilities by adding **Wazuh SIEM**
* Introduce **MITRE ATT\&CK mappings** to better classify alerts
  
---

## 🤝 How to Contribute

1. Fork the repository
2. Create a new feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a **pull request** for review

---

## Conclusion

This project demonstrates how to build a fully functional **home Security Operations Center (SOC) lab** using **UTM** and open-source tools on **macOS**. It simulates real-world attacks from Kali Linux and detects them with Suricata, then visualizes alerts using Splunk dashboards.

## Learn and practice:

* Threat detection and alerting
* Network intrusion monitoring with Suricata
* Log analysis and visualization with Splunk
* Endpoint and network telemetry correlation
* Hands-on SOC workflow design

---

## 📬 Connect with Me

* 🌐 [LinkedIn](https://www.linkedin.com/in/palakgupta2405)
* 💻 [GitHub](https://github.com/Palak-2424)

