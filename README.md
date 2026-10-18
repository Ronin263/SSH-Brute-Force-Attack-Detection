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
## 🛡️ Automated Incident Response — Fail2Ban & nftables

### Objective

Extend the detection workflow with automated response capabilities to temporarily block a source IP address after repeated failed SSH authentication attempts.

### Configuration

* **Detection tool:** Fail2Ban
* **Log source:** systemd journal
* **Firewall:** nftables
* **Failed-attempt threshold:** 5
* **Detection window:** 600 seconds
* **Ban duration:** 600 seconds

### Test Results

The response mechanism was validated in the home lab using Kali Linux as the test source and Debian as the target server.

1. Five failed SSH authentication attempts were generated.
2. Fail2Ban detected the repeated failures.
3. The source IP address was blocked through nftables.
4. The block expired automatically after approximately 10 minutes.
5. SSH connectivity was successfully restored.

### Log Evidence

```text
2026-10-18 18:46:42 — Ban 192.168.0.197
2026-10-18 18:56:42 — Unban 192.168.0.197
```

### Response Lifecycle

**Detection → Automated Blocking → Temporary Containment → Automatic Unban → Connectivity Validation**

This extension demonstrates a basic automated response workflow alongside the existing host-level, network-level, and SIEM detection components.

### Validation Status

* [x] Repeated failed SSH attempts detected
* [x] Source IP blocked through nftables
* [x] Automatic unblocking confirmed in logs
* [x] SSH connectivity restored after the ban expired


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
<img width="1400" height="371" alt="logi_ofiara" src="https://github.com/Ronin263/SSH-Brute-Force-Attack-Detection/blob/main/logi%20ofiara.jpg" />
  
* Suricata alerts
<img width="1400" height="371" alt="Suricata" src="https://github.com/Ronin263/SSH-Brute-Force-Attack-Detection/blob/main/Suricata%201.jpg?raw=true" />
<img width="1400" height="371" alt="Suricata_syg" src="https://github.com/Ronin263/SSH-Brute-Force-Attack-Detection/blob/main/sygnature%20atack.jpg?raw=true" />

* Wazuh dashboard
<img width="1400" height="371" alt="Wazuh" src="https://github.com/Ronin263/SSH-Brute-Force-Attack-Detection/blob/main/Wazuh%201.jpg?raw=true" />
<img width="1400" height="371" alt="Wazuh_1" src="https://github.com/Ronin263/SSH-Brute-Force-Attack-Detection/blob/main/Wazuh%202.jpg?raw=true" />
<img width="1400" height="371" alt="Wazuh_2" src="https://github.com/Ronin263/SSH-Brute-Force-Attack-Detection/blob/main/Wazuh%203.jpg?raw=true" />
<img width="1400" height="371" alt="Wazuh_3" src="https://github.com/Ronin263/SSH-Brute-Force-Attack-Detection/blob/main/Wazuh.jpg?raw=true" />

---

## 💬 Summary

This project simulates a real-world brute force attack and shows how it can be detected using multiple security layers. It reflects practical SOC workflows including detection, analysis, and correlation of security events.

---

## 👤 Author

[Arek]
Cybersecurity Enthusiast | SOC Analyst (aspiring)
