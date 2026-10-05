# Investigation 02 — Endpoint Telemetry: Sysmon -> Splunk

**Environment: REAL LAB.** This is not SCS SIMULATION content.

> Personal cybersecurity lab / training environment. Controlled activity against the
> author's own VMs. Not professional work experience.

**Roadmap goal:** groundwork for 6 — Malware, Windows telemetry and SIEM (Sysmon
and Splunk). Goal 6 itself has not been started.
**Topics:** SIEM (Splunk), Windows telemetry, Sysmon, endpoint process/
network visibility, host + network correlation.
**Tools:** Sysmon (v15.22, SwiftOnSecurity config), Splunk, Windows, Kali.

---

## Objective
Stand up and validate an endpoint-telemetry pipeline so host activity (the process
and account behind a network connection) is captured by Sysmon and investigable in
Splunk, then confirm it end to end with a controlled, attributable connection.

## Environment
- Windows host (Sysmon + Splunk): `LAB-WIN`
- Kali: `LAB-KALI`
- Metasploitable2: `LAB-TARGET`
- Real lab addresses and hostnames are replaced with placeholders in this write-up.
- Sysmon64 v15.22 with the SwiftOnSecurity configuration applied and validated.

## What I did
1. Confirmed Sysmon is running on the Windows host with the SwiftOnSecurity config
   (signal-focused, noise-reduced).
2. Confirmed **Event ID 3 (Network Connection)** is being collected.
3. Generated a controlled connection and verified it reached Splunk:
   - Process: PowerShell, running as the local Administrator account on `LAB-WIN`
   - Connection: `LAB-WIN` (ephemeral port) -> `LAB-KALI`:22, TCP, Initiated=true

## Observations / evidence
- Sysmon **Event ID 3** with `Initiated=true` = an **outbound** connection
  originating from the Windows host, attributed to the **process** (PowerShell) and
  the **account** (`...\Administrator`), with source/destination IP and port.
- The event was searchable in Splunk, confirming the full chain:
  **network activity -> Sysmon process/network telemetry -> Splunk investigation.**

Planned evidence files (see [`evidence/`](evidence/README.md); sanitized copies
are not yet in this repository):
- `01-sysmon-eventid3-splunk.png` — the Event ID 3 record in Splunk showing process,
  account, src/dst IP+port, and Initiated=true.
- (optional) `02-splunk-search.png` — the Splunk search used to find it.

## Analyst reasoning
Packet capture (Investigation 01) shows what crossed the wire but not *who or what*
on the host caused it. Sysmon Event ID 3 supplies that missing attribution, the
process and the account, so an analyst can tie a connection to a specific program
and user. Together the two sources give three vantage points on the same kind of
activity:
- **Nmap** — attacker's view of exposed surface
- **Wireshark / PCAP** — packet truth on the wire
- **Sysmon -> Splunk** — endpoint record (process + account + connection)

This multi-source view is the foundation for correlating an incident rather than
relying on a single log.

## Finding
A functioning endpoint-telemetry pipeline was validated: a controlled outbound
connection from the Windows host was captured by Sysmon (Event ID 3, attributed to
PowerShell / Administrator) and confirmed in Splunk. The environment is now ready to
investigate controlled network/recon activity using host telemetry.

## Next step
Re-run recon-style activity and investigate it in Splunk end to end (find the event,
state process/account/dest/port/timestamp/authorization), then correlate the Sysmon
host view with the Wireshark wire view of the same activity.
