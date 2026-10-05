# Investigation 01 — Reconnaissance / Port Scanning

**Environment: REAL LAB.** This is not SCS SIMULATION content.

> Personal cybersecurity lab / training environment. Activity performed against the
> author's own VMs on an isolated VMware network. Not professional work experience.

**Roadmap goal:** 1 — Reconnaissance and port scanning (Nmap and Wireshark).
**Topics:** reconnaissance, port scanning, ports & protocols, TCP/UDP,
common attack ports, packet analysis, TCP 3-way handshake.
**Tools:** Nmap, Wireshark, Kali Linux, Metasploitable2.

---

## Objective
Perform and then investigate network reconnaissance: enumerate a target's exposed
TCP services with Nmap, and use packet capture to understand what that activity
actually looks like on the wire, distinguishing scan behavior from a normal,
completed TCP connection.

## Environment
- Scanner / analyst: Kali — `LAB-KALI` (capture interface: eth0)
- Target: Metasploitable2 — `LAB-TARGET`
- Isolated VMware lab network; VMs confirmed to communicate.
- Real lab addresses are replaced with placeholders in this write-up.

## What I did
1. **Nmap enumeration** — from Kali: `nmap <LAB-TARGET>`. Enumerated the
   target's exposed TCP ports/services (the attacker's view of exposed surface).
2. **Captured the scan on the wire** — ran Wireshark on eth0 during the scan,
   filtered with `tcp.port == 80`, and examined the port-80 exchange.
3. **Captured a normal connection as a control** — fresh capture, then
   `curl http://<LAB-TARGET>`, filtered `tcp.port == 80`, and located the full
   handshake in the TCP stream.

## Observations / evidence
- **Scan behavior (port 80):** SYN -> SYN/ACK -> **RST**. Nmap received the SYN/ACK
  (proving the port is open) and then reset the connection instead of completing
  it. This is scan/enumeration behavior, not a normal session.
- **Normal connection (control):** SYN -> SYN/ACK -> **ACK** — a completed TCP
  3-way handshake. Client ephemeral port 37878, server port 80 (HTTP).

Planned evidence files (see [`evidence/`](evidence/README.md); sanitized copies
are not yet in this repository):
- `01-nmap-scan.png` — Nmap results: discovered open TCP ports/services.
- `02-wireshark-scan-syn-synack-rst.png` — port-80 exchange showing the RST
  (scan signature on the wire).
- `03-wireshark-tcp-3way-handshake.png` — completed SYN/SYN-ACK/ACK of a normal
  HTTP connection (the control).

## Analyst reasoning
Many SYN packets to many destination ports from one source in a short window is a
recognizable port-scanning pattern. The SYN/ACK-then-RST is the tell that the
source is probing for open ports rather than using the service. Comparing it to the
`curl` handshake makes the difference concrete: a real client completes the
handshake (ACK) and exchanges data; a scanner resets once it has learned the port
state.

Key distinctions reinforced:
- A TCP 3-way handshake is SYN -> SYN/ACK -> ACK. SYN/ACK alone does **not** prove
  completion; RST is a reset, not a completed handshake.
- Nmap answers "what can I discover?"; packet capture answers "what actually
  happened on the wire?"
- Server port 80 = HTTP service; client ports (e.g. 37878) are ephemeral.
- Packet colors in Wireshark are coloring rules, not a verdict; flags/fields/
  protocol are what matter.

## Finding
From one source host (`LAB-KALI`), a port-scan / enumeration of the target
(`LAB-TARGET`) was observed, identifying exposed TCP services. The wire evidence
(SYN/SYN-ACK/RST across ports) corroborates scanning rather than legitimate use.
In this lab the activity was authorized (self-initiated); in a real environment the
next step would be to determine authorization and scope.

## Next analyst questions (SOC workflow)
Who initiated it? What was targeted? Which ports were probed? When? Was it
authorized? What happened after services were discovered? What other telemetry
(firewall logs, IDS, endpoint) corroborates it?

## Possible follow-up (not required for Goal 1)
Disable or firewall-restrict an unnecessary exposed service on the target, re-scan,
and show the port is no longer reachable, closing the BUILD -> ... -> VERIFY loop.
