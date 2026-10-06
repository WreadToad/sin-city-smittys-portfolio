# Evidence — Lab 03 (Firewall, DMZ and perimeter defense)

**Environment: REAL LAB.**

**Status:** three redacted screenshots are in this folder. The lab is in progress
and some evidence is still missing (listed below).

| Filename | What it shows | Supports |
|---|---|---|
| `01-lan-to-dmz-ping-redacted.png` | Ping from the LAN client to the DMZ test host: 4 transmitted, 4 received, 0% packet loss | Test 1 |
| `02-dmz-to-lan-deny-log-redacted.png` | pfSense firewall log: four ICMP entries from the DMZ test host to the LAN client blocked by the default deny rule | Test 2 |
| `03-dmz-rule-before-apply-redacted.png` | The narrow DMZ rule as created. The pending-changes banner shows it was taken before the rule was applied | That the rule was created. It is not proof of the applied state |

**How these were redacted:** addresses are covered with solid boxes and replaced
with labels (LAN client, DMZ test host, external test destination). The terminal
screenshot is cropped to the ping output. Unredacted originals are kept private.

**Evidence still needed**
- Ping output from the DMZ test host for the blocked DMZ-to-LAN attempt.
- Ping output for the narrow rule after it was applied, with the received count.
- A repeat of the DMZ-to-LAN test after the rule was added.
- The rule list for each firewall interface.

**Left out on purpose:** the firewall's interface status screens. They consist
mostly of addresses, so a redacted copy would show little.

**Before adding any image here,** check that it shows no lab addresses, hardware
addresses, host or user names, local URLs, credentials or license details.
