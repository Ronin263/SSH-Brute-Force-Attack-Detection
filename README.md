# 🔐 SSH Brute Force Attack Detection using Wazuh & Suricata

## 📌 Project Overview

This project demonstrates a **realistic SSH brute force attack simulation** in a home lab environment and its detection using a **multi-layered security approach**:

* 🧠 SIEM: Wazuh
* 🌐 NIDS: Suricata
* 🖥️ Host logs: Debian (journalctl)
* ⚔️ Attack tool: Hydra (Kali Linux)
* 🛡️ Automated response: Fail2Ban
* 🔥 Firewall enforcement: nftables

The goal was to simulate an attack and validate detection across **network, host, and SIEM layers**, similar to real SOC operations.

---
## 🧪 Lab Architecture

| Component | Configuration |
|---|---|
| Attacker | Kali Linux — `192.168.0.197` |
| Target server | Debian 12 — `192.168.0.101` |
| SIEM | Wazuh |
| NIDS | Suricata 7.0.10 |
| Wazuh Agent | 4.13.1 |
| Automated response | Fail2Ban 1.0.2-2 |
| Firewall | nftables |
| Log source | systemd journal |
| Monitored service | SSH — TCP port 22 |

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

### Attack Simulation Scope

This test focused on generating repeated SSH authentication failures
using Hydra and observing the resulting events across the host,
network, and SIEM monitoring layers.

The automated Fail2Ban response was validated separately
under a dedicated test scenario described below.
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

Suricata monitored network traffic during the SSH brute-force simulation.

A custom SSH-related detection signature generated an alert
for potentially suspicious SSH activity.

The alert was treated as a network-level detection indicator
and correlated with host-based authentication logs.

Alert evidence is available in the screenshots below.

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

Data from All systems confirmed corelation the same attack:

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
* Configure firewall rules using nftables.

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

This was a separate validation scenario from the Hydra attack simulation.

The test focused on verifying the automated response to repeated
SSH authentication failures using the configured Fail2Ban SSH jail.

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

## 📚 Documentation

Detailed documentation of the project and validation scenarios:

* [Incident Report](docs/incident-report.md) — incident overview, detection, response, evidence, and recovery.
* [Test Results](docs/test-results.md) — technical test results, log correlation, and validation of the automated response workflow.
  
## 🚀 Skills Demonstrated

* SOC Analysis
* Log Analysis (Linux)
* Network Security Monitoring
* SIEM (Wazuh)
* IDS/IPS (Suricata)
* Threat Detection & Correlation
* MITRE ATT&CK framework
* Cybersecurity Lab Setup
* Automated Incident Response
* Linux Firewall Management (nftables)
* Fail2Ban Configuration and Validation
* Incident Timeline Analysis
* Security Event Investigation
---

## 📎 Screenshots
* Hydra attack
![Hydra](../main/screenshots/Kali.jpg)

* Debian logs
![Debian Logs](../main/screenshots/logi_ofiara.png)

* Suricata alerts
![Suricata alerts](../main/screenshots/network-detection/Suricata1.jpg)

* Wazuh dashboard
![Wazuh](../main/screenshots/siem-analysis/Wazuh.jpg)
![Wazuh](../main/screenshots/siem-analysis/Wazuh1.jpg)
![Wazuh](../main/screenshots/siem-analysis/Wazuh2.jpg)
![Wazuh](../main/screenshots/siem-analysis/Wazuh3.jpg)

* Fail2Ban / nftables
![Fail2Ban detection and nftables](../main/screenshots/automated-response/fail2ban-detection_nftables.png)

* Automatic unban
![Automatic unban](../main/screenshots/automated-response/automatic-unban.png)

* SSH conectivity recovery
![SSH connectivity recovery](../main/screenshots/automated-response/ssh-connections-recovery.png)


---

## 💬 Summary

This project demonstrates a multi-layered SSH brute-force detection
and automated response workflow in a controlled cybersecurity home lab.

The environment combines Wazuh for host-based security event analysis,
Suricata for network monitoring, Fail2Ban for automated response,
and nftables for firewall enforcement.

The project includes a Hydra-based SSH attack simulation and a separate
validation of automatic IP blocking, unblocking, and SSH connectivity recovery.

It demonstrates practical SOC skills in security monitoring,
log analysis, event correlation, incident response, and technical documentation.

---

## 👤 Author

[Arek]
Cybersecurity Enthusiast | SOC Analyst (aspiring)
