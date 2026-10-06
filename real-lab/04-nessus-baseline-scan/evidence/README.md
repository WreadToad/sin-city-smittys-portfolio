# Evidence — Lab 04 (Vulnerability assessment: first Nessus scan)

**Environment: REAL LAB.**

**Status:** two sanitized screenshots are in this folder. The lab is in progress
and some evidence is still missing (listed below).

| Filename | What it shows | Supports |
|---|---|---|
| `01-nessus-basic-scan-results.png` | Nessus results list: 14 entries, all rated Info, with "HTTP (Multiple Issues)" at a count of 2. Scan details: Basic Network Scan, Completed, Local Scanner, 3:18 PM to 3:28 PM, 10 minutes | That the first Basic Network Scan completed, and what it listed |
| `02-nessus-hosts-view-redacted.png` | Nessus Hosts view: Hosts 1, one row with Auth: Fail and a result bar of 15, all Info. Same scan details as image 01 | That one host was scanned and that Nessus did not log in to it |

**How these were sanitized:** both images were checked by eye and by text
recognition for lab addresses, hardware addresses, host and user names, scan
names, local URLs, credentials and license details. Image 01 shows none, so
nothing had to be covered. In image 02 the scan name and the host address are
covered with solid boxes. Both were saved again without the original file
metadata. "Ethernet MAC Addresses" in the list is the name of a Nessus check, not
an address. The unredacted original of image 02 is kept private.

**Evidence still needed**
- An exported scan report.
- A scan with credentials, for comparison.
- Any evidence of a vulnerability sweep. No sweep was performed.

**Before adding any image here,** check that it shows no lab addresses, hardware
addresses, host or user names, scan names, local URLs, credentials or license
details.
