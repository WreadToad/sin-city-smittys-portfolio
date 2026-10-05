# Evidence — Investigation 02 (Sysmon -> Splunk)

**Environment: REAL LAB.**

**Status:** no sanitized screenshot has been added to this folder yet.

Place the strategically selected screenshots here, using these exact filenames so
the writeup links resolve.

| Filename | What it should show | Evidence point |
|---|---|---|
| `01-sysmon-eventid3-splunk.png` | The Sysmon Event ID 3 record in Splunk: process (PowerShell), account, src `LAB-WIN` (ephemeral port) -> dst `LAB-KALI`:22, TCP, Initiated=true | Tool/telemetry correlation (endpoint view) |
| `02-splunk-search.png` *(optional)* | The Splunk search used to locate the event (e.g. filtering EventCode=3 + destination) | Finding (how it was found) |

**Before publishing, verify each image** has no Splunk login, credentials, tokens,
license keys, real lab IP addresses, or a real machine name tied to you. Crop or
redact lab addresses and the host name; also crop anything else sensitive (browser
tabs, other windows, license info).
