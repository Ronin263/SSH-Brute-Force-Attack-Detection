# SSH Brute Force Detection and Automated Response — Test Results

## 1. Objective

The objective was to validate the detection of repeated failed SSH authentication attempts, automated source IP blocking, automatic unblocking, and restoration of SSH connectivity.

## 2. Lab Environment

| Component              | Configuration    |
| ---------------------- | ---------------- |
| Target server          | Debian 12        |
| Test machine           | Kali Linux       |
| Target IP              | `192.168.0.101`  |
| Test source IP         | `192.168.0.197`  |
| Monitored service      | SSH, TCP port 22 |
| Detection and response | Fail2Ban         |
| Firewall enforcement   | nftables         |
| Log source             | systemd journal  |

## 3. Detection Configuration

Fail2Ban was configured to monitor SSH authentication events through the systemd journal.

* Failed-attempt threshold: 5
* Detection window: 600 seconds
* Ban duration: 600 seconds
* Jail: `sshd`

## 4. Detection and Containment

Five failed SSH authentication attempts were generated from the authorized Kali Linux test machine.

Fail2Ban detected the repeated failures and blocked the source IP address through nftables.

The Fail2Ban status reported:

* Total failed attempts: 5
* Total bans: 1
* Currently banned: 1 during the active block

The nftables ruleset confirmed that the source IP was present in the Fail2Ban-managed address set.

## 5. Automatic Unblocking

The Fail2Ban logs recorded the following events:

```text
2026-10-18 18:46:42 — Ban 192.168.0.197
2026-10-18 18:56:42 — Unban 192.168.0.197
```

The source IP was unblocked approximately 10 minutes after the ban was applied, consistent with the configured 600-second ban duration.

After the ban expired, the Fail2Ban status reported zero currently banned addresses, and the nftables address set was empty.

## 6. Recovery Validation

After the automatic unban, an SSH connection from Kali Linux to the Debian server was successfully established.

This confirmed that SSH connectivity was restored after the temporary block expired.

## 7. Results

| Test                                        | Result |
| ------------------------------------------- | ------ |
| Generate failed SSH authentication attempts | PASS   |
| Detect repeated authentication failures     | PASS   |
| Block the source IP using nftables          | PASS   |
| Confirm the active ban                      | PASS   |
| Automatically remove the ban                | PASS   |
| Restore SSH connectivity                    | PASS   |

## 8. Conclusion

The test validated a basic automated response workflow for repeated failed SSH authentication attempts using Fail2Ban and nftables.

The lab demonstrated detection, temporary containment, automatic unblocking, and post-response connectivity validation.

The results apply to the tested home-lab configuration and do not, by themselves, establish production-scale effectiveness or false-positive performance.
