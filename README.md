# Sin City Smitty's — Cybersecurity Lab and SOC Training Portfolio

Sin City Smitty's (SCS) is a personal cybersecurity training and portfolio
project. I use it to build and document hands-on security skills against a fixed
12-goal roadmap, from reconnaissance through cloud identity.

> **Personal training project.** All hands-on activity was performed on lab
> virtual machines I own, on an isolated lab network, with my own authorization.
> "Sin City Smitty's" is a fictional casino/resort. It is not affiliated with any
> employer or real organization, and it does not model or represent any company's
> systems or infrastructure. This is not professional work experience.

## Learning purpose

I hold CompTIA Security+ and am working toward a B.S. in Cybersecurity (WGU, in
progress). This project turns that study into evidence: each roadmap goal is
practiced in a real lab, investigated the way a SOC analyst would, and written up
with what was done, what was observed, and what it demonstrates.

## Two separate environments

This project has two environments. They are kept separate and always labeled.

| | REAL LAB | SCS SIMULATION |
|---|---|---|
| What it is | An isolated VM lab on my own hardware: a Kali Linux analyst VM, a Windows host running Sysmon and Splunk, a Metasploitable2 target, and Nessus Essentials | A software model of a fictional casino/resort enterprise: a TypeScript simulation core, PostgreSQL, and a 3D client |
| Where the data comes from | Real tools, real packets and real logs | Simulated systems reacting to simulated activity |
| What it is for | Producing the skills evidence in this portfolio | A learning aid for visualizing network topology, traffic paths and how telemetry is produced |
| Evidence in this repository | Yes, under [`real-lab/`](real-lab/) | None at this checkpoint |

Simulated output is never presented as real-lab evidence. Any SCS SIMULATION
content added later will be labeled as simulated and stored apart from `real-lab/`.

## Roadmap: 12 goals

The project is scoped to these 12 goals. They are the finish line.

| # | Goal | Tools | Status |
|---|---|---|---|
| 1 | Reconnaissance and port scanning | Nmap, Wireshark | Complete (REAL LAB) |
| 2 | Vulnerability assessment | Nessus | In progress: Nessus installed, registered and initialized; first scan not yet run |
| 3 | Firewall, DMZ and perimeter defense | | Next |
| 4 | IDS/IPS and network detection | | Not started |
| 5 | Web-based attacks | Burp | Not started |
| 6 | Malware, Windows telemetry and SIEM | Sysmon, Splunk | Not started; telemetry groundwork in place (see investigation 02) |
| 7 | Linux security | | Not started |
| 8 | Credential access | Mimikatz | Not started |
| 9 | Active Directory and identity | BloodHound | Not started |
| 10 | Lateral movement | PsExec | Not started |
| 11 | Attack lifecycle and MITRE ATT&CK | | Not started |
| 12 | Cloud identity | Azure, Entra, Sentinel | Not started |

## Completed REAL LAB milestones

- Authorized lab reconnaissance with Nmap, including port and service discovery
  against the lab target (Goal 1).
- Wireshark packet and TCP analysis, including observation of the TCP three-way
  handshake and how it differs from scan traffic (Goal 1).
- Sysmon configured on the Windows lab host with the SwiftOnSecurity
  configuration (groundwork for Goal 6).
- Real Sysmon Event ID 3 (network connection) telemetry generated in the lab and
  observed in Splunk (groundwork for Goal 6).
- Nessus Essentials 10.12.5 (x64) installed, registered and initialized (Goal 2).

**Nessus status:** the first scan has not been run. There are no scan results,
findings, vulnerabilities or remediation to report yet.

## Selected evidence

Two REAL LAB investigations are written up. Each is short and states only what
was observed.

| Investigation | What was done | What was observed | Skills demonstrated |
|---|---|---|---|
| [01 — Reconnaissance and port scanning](real-lab/01-recon-port-scanning/README.md) | Scanned the lab target with Nmap, captured the scan in Wireshark, then captured a normal HTTP connection as a control | Scan traffic to port 80 ended SYN, SYN/ACK, RST; the normal connection completed SYN, SYN/ACK, ACK | Port and service discovery, packet analysis, telling scan behavior from a normal TCP session |
| [02 — Endpoint telemetry: Sysmon to Splunk](real-lab/02-sysmon-splunk-telemetry/README.md) | Applied the SwiftOnSecurity Sysmon configuration, generated a controlled outbound connection, and searched for it in Splunk | A Sysmon Event ID 3 record in Splunk naming the process, the account, and the source and destination of the connection | Endpoint telemetry setup, SIEM validation, attributing network activity to a process and account |

Screenshots are limited to a few per investigation. Sanitized copies have not
been added to this repository yet; each investigation's `evidence/` folder lists
the planned images and their status.

## Lab environment (sanitized)

| Role | System | Placeholder used in these documents |
|---|---|---|
| Analyst / scanner | Kali Linux VM | `LAB-KALI` |
| Windows host (Sysmon and Splunk) | Windows VM | `LAB-WIN` |
| Vulnerable target | Metasploitable2 VM | `LAB-TARGET` |

The lab runs on an isolated VMware network. Real lab IP addresses, hostnames and
account names are intentionally left out of this repository.

## Evidence and sanitization policy

- A few selected screenshots per investigation, not a dump. Each one has a
  stated purpose.
- REAL LAB evidence is kept separate from SCS SIMULATION content.
- Public documents and images contain no real lab IP addresses, hostnames,
  usernames, credentials, tokens, license keys or personal information.
  Placeholders are used instead.
- Nothing from an employer or any other organization is included.

## About the SCS SIMULATION

The simulation models a fictional casino/resort enterprise so that network,
identity and telemetry concepts can be seen working together. Its one rule is
that the simulation core is the source of truth: logs are produced by simulated
systems reacting to simulated activity and are never written by hand.

At this checkpoint the foundation exists (a first end-to-end slice and a modeled
enterprise network topology). It has no attack, detection or scenario engine.
The simulation code and database are maintained separately and are not part of
this repository at this checkpoint.

## Future work (outside the 12 goals)

These ideas are recorded so they are not lost. They are not current
requirements and do not change the roadmap above.

- Forwarding simulated telemetry into Splunk.
- Expanding the simulated enterprise and its 3D visualization.
- Scenario, detection and learning-progress features in the simulation.

## Repository layout

```
README.md
real-lab/
  01-recon-port-scanning/
    README.md
    evidence/README.md
  02-sysmon-splunk-telemetry/
    README.md
    evidence/README.md
```
