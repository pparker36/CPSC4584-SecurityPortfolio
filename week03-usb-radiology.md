# Week 3: Unauthorized USB Drive in Radiology
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** September 14, 2026
**Analyst:** [Pierre Parker]
**Incident ID:** INC-2026-0914-001

---

## Incident Summary

[An unmarked USB drive was found plugged into workstation MHS-RAD-WS-03 in the Radiology imaging suite by a facilities technician at 11:47 AM. 
IT logged and tagged the device as MHS-USB-2026-0913-001 and isolated the workstation without opening any files]

---

## Chain of Custody

[Maintaining a strict chain of custody is super important because it proves the evidence wasn't messed with or contaminated. 
In this case, proper logs tracked every transfer from the technician to the IT helpdesk and straight into SOC custody]

---

## Key Encoding Finding

**String Found:** dW5hdXRob3JpemVkIGFjY2Vzcwo=
**Encoding Type:** [Base64]
**Decoded Content:** [curl -s -o /dev/nullpparker3-academy@webshell:~$=]
**Significance:** [It decodes to a silent web request command fragment, which looks suspicious, but we'd need more context or logs to prove malicious activity]

---

## Terminal Commands Used

| Command | Purpose |
|---------|---------|
| echo "..." \| base64 | [Encodes text into Base64 to show how it scrambles data without real encryption.] |
| echo "..." \| base64 -d | [Decodes Base64 strings back into readable text so you can check what they say.] |
| xxd .bashrc \| head -6 | [Shows hex bytes and headers to inspect low-level file structure instead of just trusting extensions] |
| strings .bashrc \| grep -i "..." | [Filters out binary noise to quickly isolate key terms and configuration patterns.] |

---

## Escalation Recommendation

As a Tier 1 analyst, I definitely recommend escalating this to Tier 2 because an unknown USB drive was plugged into a sensitive hospital EHR workstation. 
We still need a forensic expert to look at the drive contents and check the six staff members with keycard access during the time period
---
*CPSC 4584 | Governors State University | Fall 2026*
    
