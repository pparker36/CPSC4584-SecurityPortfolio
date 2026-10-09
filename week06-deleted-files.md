# Week 6: Deleted Files on a Terminated Employee's Laptop

**Course:** CPSC 4584 | Special Topics in Information Security  
**Date:** October 5, 2026  
**Analyst:** Pierre Parker  
**Case ID:** FOR-2026-1005-001

---

## Incident Summary

On October 3, 2026, Maplewood Health System terminated billing coordinator Jordan Ellis, but his company laptop was not collected when he left. The laptop was returned on October 5, and investigators later recovered five file artifacts from unallocated disk space, including billing-related files and one partial artifact.

## Chain of Custody Status

**Gap Period:** October 3, 2:47 PM to October 5, 9:30 AM (about 43 hours)  
**Documented By:** Analyst during device intake

**Gap Significance:** The laptop stayed outside Maplewood's control during this time, so investigators cannot fully verify who accessed it or what happened to it before it was returned. File deletion events were recorded between 11:30 PM on October 3 and 12:04 AM on October 4, but those timestamps alone do not prove who performed the actions or why. I would clearly document this gap so anyone reviewing the case understands the limits of the evidence.

## Recovered File Evidence Classification

| Filename | Type | Evidentiary Value | Finding |
|---|---|---|---|
| `insurance_export_final.xlsx` | Spreadsheet | High priority for review | Last modified October 3 at 11:47 PM and deleted at 11:52 PM. The name suggests a possible insurance export, but the contents and any transfer activity would need to be verified. |
| `mhs_billing_schema_db.sql` | SQL script | High priority for review | Last modified October 1 at 9:14 AM and deleted October 3 at 11:53 PM. It may relate to Maplewood's billing database structure, but the filename alone does not establish what information it contains. |
| `vendor_contact_list_external.docx` | Document | Relevant for follow-up | Last modified October 2 at 3:22 PM and deleted October 3 at 11:55 PM. The name raises questions about vendor contacts, but it does not prove anyone shared the file externally. |
| `__tmp_8f2a.dat` | Partial artifact | Limited | Only about 22 KB of an approximately 400 KB file was recovered. Its timestamps were unreadable, so there is not enough information to reliably identify its original purpose or activity. |
| `personal_vacation_2025.jpg` | Image | Lower priority based on current evidence | Last modified July 14, 2025, and deleted October 3 at 11:58 PM. It appears personal based on the filename, and its connection to the investigation has not been established. |

These are preliminary priorities based on the available file metadata, not conclusions about misconduct or the contents of the files.

## Metadata Inspection Commands Used

| Command | Purpose |
|---|---|
| `stat .bashrc` | I checked the file's size, permissions, owner, inode, and Access, Modify, Change, and Birth timestamps. All four timestamps in my practice output showed September 3, 2026, at 22:07:05 UTC. |
| `ls -lai` | I viewed filenames alongside their inode numbers and permissions. My `.bashrc` file showed inode `740964797`, which matched the `stat` output. |
| `xxd .bashrc \| head -3` | I used this to inspect the first few lines of a practice file in hexadecimal and readable text. File signatures can help identify file types during recovery, although I have not included the exact byte output here. |
| `find . -newer .bashrc -type f` | I searched for regular files modified after `.bashrc`. The results included `README.txt`, `access-review.txt`, `recovery-note.txt`, and other practice files. This was a relative-time search, not a search limited to the Maplewood incident period. |

These commands were run on practice files in the CyLab Webshell, not directly on the recovered Maplewood laptop image.

## Escalation Summary

The main issue I would report is the approximately 43-hour gap between Jordan Ellis leaving Maplewood and returning the laptop. Investigators documented file deletion activity during that period. After the laptop was returned, a forensic analyst created an image using FTK Imager on October 5 at 10:15 AM and verified the image's SHA-256 hash against the source drive. At 11:42 AM, the analyst used Foremost to recover five file artifacts from unallocated space. Four were listed as completely recovered, while one was only partially recovered.

I would prioritize the insurance export and billing database script for further review because they may relate to sensitive business operations. However, recovered filenames, file timestamps, and the verified forensic image do not prove who deleted the files, whether they were copied elsewhere, or what anyone intended. The verified hash supports the integrity of the forensic image at the time of analysis, but it does not fix the earlier custody gap.

My next step would be to send a factual report through my supervisor for authorized HR and legal review. They can decide whether further investigation is needed, such as checking device activity, account logs, and possible file transfers. As a Tier 1 analyst, my job is to accurately document technical findings and their limits, not contact the former employee or determine misconduct or liability.

---

*CPSC 4584 | Governors State University | Fall 2026*
