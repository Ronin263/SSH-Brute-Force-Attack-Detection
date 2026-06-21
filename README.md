# 🔐 SSH Brute Force Attack Detection using Wazuh & Suricata

## 📌 Project Overview

This project demonstrates a **realistic SSH brute force attack simulation** in a home lab environment and its detection using a **multi-layered security approach**:

* 🧠 SIEM: Wazuh
* 🌐 NIDS: Suricata
* 🖥️ Host logs: Debian (journalctl)
* ⚔️ Attack tool: Hydra (Kali Linux)

The goal was to simulate an attack and validate detection across **network, host, and SIEM layers**, similar to real SOC operations.

---

## 🧪 Lab Architecture

* **Attacker:** Kali Linux (192.168.0.197)
* **Target:** Debian Server (192.168.0.101)
* **SIEM:** Wazuh
* **NIDS:** Suricata

---

## ⚔️ Attack Simulation

The attack was performed using **Hydra**:

```bash
hydra -l root -P rockyou.txt ssh://192.168.0.101
```

### 📊 Attack Details:

* ⏱️ Duration: ~10 minutes (19:30 – 19:40)
* 🔁 Attempts: ~2000 login attempts
* 🎯 Target: SSH (port 22)
* 👤 Account targeted: root

---

## 🔍 Detection Results

### 🖥️ Host-Level Detection (Debian)

* Logs collected via `journalctl`
* Multiple entries:

  ```
  Failed password for root from 192.168.0.197
  ```
* Confirms brute force activity at system level

---

### 🌐 Network-Level Detection (Suricata)

* Detected high-frequency SSH traffic
* Alerts generated based on threshold rules
* Logs available in:

  * `fast.log`
  * `eve.json`

Example alert:

```
ET SCAN SSH Brute Force Attempt
```

---

### 🧠 SIEM Detection (Wazuh)

* Detected multiple failed login attempts
* Triggered rules for:

  * Authentication failure
  * Password guessing

### 🔥 MITRE ATT&CK Mapping:

* **T1110 – Brute Force**
* Category: Credential Access

---

## 🔗 Correlation Analysis

All systems confirmed the same attack:

| Indicator | Value         |
| --------- | ------------- |
| Source IP | 192.168.0.197 |
| Target IP | 192.168.0.101 |
| Port      | 22 (SSH)      |
| Time      | 19:30 – 19:40 |

### ✅ Conclusion:

The attack was successfully detected across:

* Host logs
* Network IDS
* SIEM platform

This demonstrates a **defense-in-depth security model**.

---

## 🛡️ Mitigation Recommendations

* Disable SSH root login
* Use SSH key authentication
* Install and configure fail2ban
* Limit login attempts
* Change default SSH port
* Enable firewall rules (iptables / ufw)

---

## 🚀 Skills Demonstrated

* SOC Analysis
* Log Analysis (Linux)
* Network Security Monitoring
* SIEM (Wazuh)
* IDS/IPS (Suricata)
* Threat Detection & Correlation
* MITRE ATT&CK framework
* Cybersecurity Lab Setup

---

## 📎 Screenshots
* Hydra attack
<img width="1400" height="371" alt="Kali" src="https://github.com/user-attachments/assets/74797b41-828f-4712-abc6-d96927b4acba" />

* Debian logs
  




* Suricata alerts
* Wazuh dashboard

---

## 💬 Summary

This project simulates a real-world brute force attack and shows how it can be detected using multiple security layers. It reflects practical SOC workflows including detection, analysis, and correlation of security events.

---

## 👤 Author

[Your Name]
Cybersecurity Enthusiast | SOC Analyst (aspiring)
