# Incident Report: SSH Brute-Force Attempt

## 1. Incident Overview

- **Incident type:** SSH brute-force attempt
- **Severity:** Medium (lab assessment)
- **Target:** Debian 12 server
- **Target IP:** `192.168.0.101`
- **Source IP:** `192.168.0.197` (Kali Linux)
- **Affected service:** SSH (TCP/22)
- **Detection and response tool:** Fail2Ban
- **Firewall:** nftables
- **Status:** Resolved

## 2. Executive Summary

Repeated failed SSH authentication attempts were generated from a controlled Kali Linux test machine against a Debian server.

Fail2Ban detected five failed authentication attempts and automatically blocked the source IP address using nftables.

The block expired after approximately 10 minutes. After the ban expired, an SSH connection from Kali Linux to the Debian server was successfully established.

The test validated the detection, temporary blocking, automatic unblocking, and recovery stages of the response workflow.

## 3. Detection

Fail2Ban monitored SSH authentication events through the systemd journal.

The SSH jail used the following configuration:

- **Maximum failed attempts:** 5
- **Detection window:** 600 seconds
- **Ban duration:** 600 seconds
- **Log source:** systemd journal
- **Jail:** sshd

The Fail2Ban status confirmed five failed attempts and one ban.

## 4. Response and Containment

After the configured threshold was reached, Fail2Ban blocked the source IP address using nftables.

During the active ban, SSH traffic from the blocked address was rejected by the firewall rule managed by Fail2Ban.

The nftables ruleset confirmed that the source IP address was added to the Fail2Ban-managed address set.

## 5. Incident Timeline

| Time | Event |
|---|---|
| 18:46:42 | Fail2Ban blocked 192.168.0.197 |
| 18:56:42 | Fail2Ban automatically unblocked 192.168.0.197 |
| After unblocking | SSH connectivity from Kali Linux to Debian was successfully restored |

The timeline is based on the Fail2Ban logs collected during the test on October 18, 2026.

## 6. Evidence

The following entries were collected from the Fail2Ban logs:

- **Ban event:** 18:46:42 — `Ban 192.168.0.197`
- **Unban event:** 18:56:42 — `Unban 192.168.0.197`

These entries confirm that Fail2Ban applied the ban and subsequently removed it approximately 10 minutes later.

The Fail2Ban status after the ban expired showed:

- **Currently failed:** 0
- **Total failed:** 5
- **Currently banned:** 0
- **Total banned:** 1

The nftables address set was empty after the ban expired, confirming that the source IP was no longer blocked by this Fail2Ban action.

## 7. Recovery and Validation

After the automatic unban, a new SSH connection from Kali Linux to the Debian server was successfully established.

This confirmed that SSH connectivity was restored after the temporary block expired.

The recovery test completed the validation of the automated response workflow.

## 8. Root Cause and Assessment

The failed authentication attempts were intentionally generated as part of an authorized security test in a controlled home lab.

The experiment was designed to validate the detection and response mechanisms rather than investigate a real-world compromise.

The test demonstrated that Fail2Ban could detect repeated failed SSH authentication attempts and enforce a temporary IP block through nftables.

No successful unauthorized login was demonstrated by this test.

## 9. Recommendations

The following measures can further improve SSH security:

- Use SSH key-based authentication where appropriate.
- Disable password authentication when operationally feasible.
- Restrict SSH access to trusted networks where possible.
- Monitor SSH authentication events and review suspicious activity.
- Integrate host and network telemetry with Wazuh and Suricata.
- Review Fail2Ban events and investigate potential false positives.
- Retest the response mechanism after configuration changes.

## 10. Final Status

**Result: PASS**

The following capabilities were successfully validated:

- Detection of repeated failed SSH authentication attempts.
- Automatic blocking of the source IP through nftables.
- Automatic unblocking after the configured ban duration.
- Restoration of SSH connectivity after the ban expired.

This project demonstrates a basic automated incident response workflow in a controlled cybersecurity home lab using Fail2Ban and nftables.
