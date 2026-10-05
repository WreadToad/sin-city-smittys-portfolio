# Evidence — Investigation 01 (Reconnaissance / Port Scanning)

**Environment: REAL LAB.**

**Status:** the three screenshots below were captured in the lab. Sanitized copies
have not been added to this folder yet.

Place the strategically selected screenshots here, using these exact filenames so
the writeup links resolve. Keep to these few; do not add every screenshot.

| Filename | What it should show | Evidence point |
|---|---|---|
| `01-nmap-scan.png` | Nmap output for `nmap <LAB-TARGET>` — the discovered open TCP ports/services | Discovery / attack signature |
| `02-wireshark-scan-syn-synack-rst.png` | Wireshark on eth0, filter `tcp.port == 80`, the SYN -> SYN/ACK -> RST exchange from the scan | Tool/telemetry correlation (wire view of the scan) |
| `03-wireshark-tcp-3way-handshake.png` | Wireshark, filter `tcp.port == 80`, the completed SYN -> SYN/ACK -> ACK (client port 37878 -> server 80) from the `curl` control | Finding (normal vs. scan contrast) |

Optional later:
- `00-baseline.png` — normal/quiet traffic before the scan (baseline).
- `04-verification.png` — re-scan after restricting a service, showing the port now closed (remediation/verification).

**Before publishing, verify each image** has no credentials, password prompts,
tokens, logins, real lab IP addresses, or a real machine name tied to you. Crop or
redact lab addresses and hostnames.
