# Lab 03 — Firewall, DMZ and perimeter defense (in progress)

**Environment: REAL LAB.** This is not SCS SIMULATION content.

> Personal cybersecurity lab / training environment. Everything here runs on
> virtual machines I own, on isolated virtual networks. Not professional work
> experience. The hands-on work for this goal is in progress, not complete.

**Roadmap goal:** 3 — Firewall, DMZ and perimeter defense.
**Topics:** firewalls, stateful filtering, DMZ (demilitarized zone), network
segmentation, default deny, reading firewall logs.
**Tools:** pfSense CE (Community Edition) 2.7.2, ping, the pfSense firewall log.
**Status:** In progress. A firewall with separate LAN and DMZ segments is built
and the first ping tests are recorded with screenshots. Port-level tests and the
full rule list are still to do.

---

## Objective
Build a firewall myself, split one flat lab network into separate segments, place
a DMZ, and show with tests and logs which traffic is allowed and which is blocked.
This is my first firewall build.

## Lab topology (sanitized)

```
          outside network (hypervisor NAT)
                        |
                 [ pfSense firewall ]
                  /                \
         LAN segment            DMZ segment
         LAN client             DMZ test host
```

- The firewall is a virtual machine with three network adapters, one per network.
- The LAN and the DMZ are separate virtual networks, each with its own subnet.
- The LAN client and the DMZ test host get their addresses from the firewall.
- Real addresses, interface names and host names are left out of this write-up.

## Firewall policy at this stage

| Path | Result | Basis |
|---|---|---|
| LAN client to DMZ test host | Allowed | Test 1 |
| DMZ test host to LAN client | Denied by the firewall's default deny rule | Test 2 and the firewall log |
| DMZ test host to the outside | One narrow rule added: ICMP (Internet Control Message Protocol) only, from the DMZ test host only, to one external test destination only | Test 3 |

The narrow rule is not general internet access for the DMZ. I have not yet
captured the full rule list for each interface.

## Tests and results

| # | Test | Result | Evidence | Status |
|---|---|---|---|---|
| 1 | Ping from the LAN client to the DMZ test host | 4 transmitted, 4 received, 0% packet loss | Screenshot 01 | Supported by screenshot |
| 2 | Ping from the DMZ test host to the LAN client (first attempt) | Denied. The firewall log shows four ICMP entries from the DMZ test host to the LAN client blocked by the default deny rule | Screenshot 02 | Supported by the firewall log. The ping output for this first attempt was not captured |
| 3 | Added a narrow DMZ rule for ICMP to one external test destination, applied it, then pinged that destination from the DMZ test host | 4 transmitted, 4 received, 0% packet loss | Screenshot 03 (the rule as created, before it was applied) and screenshot 04 (the ping) | Supported by screenshot 04 |
| 4 | Ping from the DMZ test host to the LAN client again, with the rule in effect. Run twice | 4 transmitted, 0 received, 100% packet loss, both times | Screenshot 04 | Supported by screenshot 04 |

**How the order of tests 3 and 4 is known**
- Screenshot 04 is one console window on the DMZ test host. It shows the commands
  in order: the ping to the external test destination succeeded, then two pings
  to the LAN client failed.
- The screenshot has no timestamps. Screenshot 03 was taken before the rule was
  applied, and no screenshot shows the rule list after it was applied.
- Screenshot 03 shows this rule as the only rule on the DMZ interface. An earlier
  attempt to reach the external test destination, before the rule existed, failed
  (observed, not captured). So 4 of 4 replies means the rule was in effect, and
  the two LAN pings that follow it came after the rule was applied.
- The 100% loss matches the default deny rule still blocking DMZ-to-LAN traffic.
  The log in screenshot 02 cannot be tied to these two specific attempts.

## Evidence

Redacted copies only. Addresses are replaced with labels.

**01 — LAN client to DMZ test host: 4 of 4 replies**

![Ping from the LAN client to the DMZ test host, 4 transmitted and 4 received](evidence/01-lan-to-dmz-ping-redacted.png)

**02 — Firewall log: DMZ test host to LAN client blocked by the default deny rule**

![Firewall log showing four denied ICMP entries from the DMZ test host to the LAN client](evidence/02-dmz-to-lan-deny-log-redacted.png)

**03 — The narrow DMZ rule as created (taken before the change was applied)**

![Firewall rule on the DMZ interface allowing ICMP from the DMZ test host to one external test destination, with the pending-changes banner still showing](evidence/03-dmz-rule-before-apply-redacted.png)

**04 — DMZ test host: external ping 4 of 4, then two LAN pings 0 of 4**

![Console on the DMZ test host showing a ping to the external test destination with 4 of 4 replies, followed by two pings to the LAN client with 100% packet loss](evidence/04-dmz-host-pings-after-rule-redacted.png)

See the [evidence notes](evidence/README.md) for what each image shows and what
is still missing.

## What this shows, and what it does not

**Shows**
- A real firewall now separates a LAN segment and a DMZ segment in the lab.
- A host on the LAN could reach the DMZ test host.
- When the DMZ test host tried to reach the LAN client, the firewall blocked it
  and logged each attempt.
- With one narrow rule in effect, the DMZ test host reached the single external
  test destination (4 of 4).
- With that rule in effect, the DMZ test host still could not reach the LAN
  client (0 of 4, twice). The narrow rule did not open a path into the LAN.

**Does not show yet**
- The rule list after the change was applied. Screenshot 03 predates applying it.
- Firewall log entries tied to the two later DMZ-to-LAN attempts.
- Anything beyond ping. Port-level tests and packet captures are still to do.
- Hands-on experience with a next-generation firewall (NGFW). I have not used
  one. NGFW concepts are part of my study for this goal only.

## Why this matters to a business (SCS SIMULATION context)

The SCS SIMULATION is a separate, fictional environment: a made-up casino and
resort. It does not represent any real company's systems.

In a resort like that, the public guest website has to be reachable from the
internet, while the systems that run sales, staff applications and guest data
do not. Putting the public-facing server in a DMZ means that if it is ever
compromised, it cannot start connections into the internal network. A narrow
outbound rule follows the same idea: allow the one path a system needs and
nothing broader.

The simulation models its own firewall rules for that fictional network. Those
rules are simulated. They do not enforce the real pfSense rules in this lab, and
this lab does not change the simulation.

## Next steps
- Capture the rule list for each interface as applied, and write down why each
  rule exists.
- Repeat the allow and block tests with Nmap and Wireshark.
- Draw the before and after network diagram.
