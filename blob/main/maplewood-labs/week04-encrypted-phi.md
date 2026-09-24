# Week 4: Unencrypted Patient Records on a Shared Drive
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** September 21, 2026
**Analyst:** [Pierre Parker]
**Audit ID:** AUD-2026-0921-001

---

## Incident Summary

[During a routine IT audit, we found a misconfigured folder called PATIENT_DATA_ARCHIVE sitting right on the Maplewood clinical file server with 847 files and 8,247 unique patient records completely exposed in plaintext. 
The folder was last touched back on March 14, 2024, and was wide open with read and write access for anyone on the clinical network.]

---

## HIPAA Compliance Assessment

| Requirement | Status | Finding |
|-------------|--------|---------|

| Encryption at Rest | REQUIRES REVIEW | [When HIPAA treats encryption as an addressable implementation specification rather than a blanket requirement, it really tells us that it's not a total free pass to ignore it; 
organizations just have to evaluate if it makes sense for their setup, document their decision, and use an equivalent safeguard if they skip it] |

| Access Controls | CONTROL FAILURE | [Read and write permissions were set up way too broadly across the whole clinical network instead of being restricted only to folks whose actual job duties required touching that archived PHI.] |

| Audit Controls | CONTROL FAILURE | [There were zero access logs kept for the folder before we found it, meaning we have no way to reconstruct who looked at the archived PHI or when they did it.] |

---

## Cryptographic Controls Evaluated

**Base64 encoding:** [If the IT team tries to claim that Base64 encoding is protecting those files, I’d classify that as weak data obfuscation rather than real security. 
Base64 isn't encryption at all since it’s just a standard encoding format with zero secret keys—anyone can instantly decode it back to plaintext with a single click.]

**Caesar cipher:** [A Caesar cipher is basically an ancient classical cipher with only 25 possible shifts, meaning it offers zero modern cryptographic protection and can be brute-forced or cracked by hand in seconds.]

**Modern encryption at rest:** [Since neither uses a proper cryptographic key, Maplewood can't rely on these gimmicks and definitely needs to evaluate a real, recognized encryption method (like AES) or other reasonable safeguards to actually protect stored ePHI from being easily exposed.]

---

## Hashing Commands Practiced

| Command | Purpose | Output Length |
|---------|---------|---------------|
| echo -n "..." \| sha256sum | [This shows the avalanche effect; tweaking even one single letter totally scrambles about half of the resulting hex characters, 
which is awesome for integrity checks even though hashes don't tell you why something changed] | [64 characters (256 bits)]|

| echo -n "..." \| md5sum | [MD5 is way too weak for collision resistance nowadays, 
but security analysts still run into it in legacy systems, old file catalogs, or existing threat intelligence databases.] | [32 characters (128 bits)] |

| sha256sum .bashrc | [A forensic analyst can hash a source file before copying it and check the new file afterward to prove the copy matches perfectly without corruption, 
all while keeping the original evidence completely untouched to preserve the chain of custody.] | [64 characters (256 bits)] |

---

## Escalation Summary

[My escalation report to the CISO breaks down the discovery of that unencrypted PATIENT_DATA_ARCHIVE folder on the Maplewood file server under audit ref AUD-2026-0921-001, which contains 847 files with 8,247 patient records including SSNs and diagnoses last modified on March 14, 2024. 
The report calls out major control failures like overly permissive read/write access across the network and missing access logs that make historical tracking impossible. It highlights that no file or disk encryption was applied, meaning leadership 
and legal need to step in to review addressable encryption requirements and handle the breach determination assessment. Finally, as a Tier 1 analyst, 
I made sure to avoid jumping to conclusions about a confirmed data exfiltration because we still don't know the full scope of unauthorized access.]

---
*CPSC 4584 | Governors State University | Fall 2026*
