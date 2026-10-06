# Evidence — Lab 03 (Firewall, DMZ and perimeter defense)

**Environment: REAL LAB.**

**Status:** four redacted screenshots are in this folder. The lab is in progress
and some evidence is still missing (listed below).

| Filename | What it shows | Supports |
|---|---|---|
| `01-lan-to-dmz-ping-redacted.png` | Ping from the LAN client to the DMZ test host: 4 transmitted, 4 received, 0% packet loss | Test 1 |
| `02-dmz-to-lan-deny-log-redacted.png` | pfSense firewall log: four ICMP entries from the DMZ test host to the LAN client blocked by the default deny rule | Test 2 |
| `03-dmz-rule-before-apply-redacted.png` | The narrow DMZ rule as created. The pending-changes banner shows it was taken before the rule was applied | That the rule was created. It is not proof of the applied state |
| `04-dmz-host-pings-after-rule-redacted.png` | Console on the DMZ test host, in order: ping to the external test destination, 4 transmitted, 4 received; then two pings to the LAN client, each 4 transmitted, 0 received | Tests 3 and 4 |

**Timing of screenshot 04:** it has no timestamps. The order inside it is clear,
and the external ping could only succeed with the narrow rule in effect, so the
two LAN pings after it were run after the rule was applied. The write-up explains
this in full.

**How these were redacted:** addresses are covered with solid boxes and replaced
with labels (LAN client, DMZ test host, external test destination or EXT-DEST).
The terminal screenshots are cropped to the ping output. In screenshot 04 the
shell prompt is covered because it shows an account and host name, and one
earlier summary line with no visible target was cropped off the top. Unredacted
originals are kept private.

**Evidence still needed**
- The rule list for each firewall interface, as applied.
- Firewall log entries for the two later DMZ-to-LAN attempts.
- Port-level tests and a packet capture.

**Left out on purpose:** the firewall's interface status screens. They consist
mostly of addresses, so a redacted copy would show little.

**Before adding any image here,** check that it shows no lab addresses, hardware
addresses, host or user names, local URLs, credentials or license details.
