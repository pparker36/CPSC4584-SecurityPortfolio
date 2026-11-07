# Week 10: Suspicious Executable Running on a Server

**Course:** CPSC 4584 | Special Topics in Information Security  
**Date:** November 7, 2026  
**Analyst:** Pierre Parker  
**Case ID:** EXEC-2026-1030-001

---

## Incident Summary

On October 30, 2026, the Maplewood team found an unknown program called `mhs_monitor` running on the EHR application server for at least 72 hours. What makes it more concerning is that the process was making repeated connections to `185.220.101.47`, the same IP address identified in the Week 7 network anomaly.

---

## Static Analysis Findings

| # | Tool | Output Line | Finding | Severity |
|---|------|-------------|---------|----------|
| 1 | `strings` | `185.220.101.47` | A specific IP address was found inside the program, and it matches the earlier network incident. | High |
| 2 | `strings` | `/tmp/.mhs_session` | A hidden-looking temporary file path was found. It could be used to store information, but we do not know its contents yet. | Medium |
| 3 | `strings` | `Mozilla/5.0 (compatible; MHSMonitor/1.0)` | A browser-like user-agent string was found. It might be used in HTTP requests, but that has not been confirmed. | Medium |
| 4 | `readelf` | `[NEEDED] libcurl.so.4` | The program depends on a library that supports network requests. This is worth checking alongside the observed connections. | Medium |

I think these findings matter more when we look at them together. A library or string by itself does not prove the program is harmful, but the matching IP address and unexplained activity give the team a reason to investigate further. The `file` command also identified the sample as a stripped, dynamically linked 64-bit ELF executable, which can make parts of the investigation harder.

---

## Process-Level IOCs

| IOC | Observed Value | Significance |
|-----|----------------|--------------|
| Not in inventory | No package manager entry | I think this is concerning because the software is not listed as an approved installation. It does not prove it is malware, but the team should find out why it is there. |
| Parent process | `init (PID 1)` | This shows which process is its parent, but it does not tell us who installed or launched it originally, or whether it is malicious. There is also no documented service entry. |
| Repeated outbound connection | `185.220.101.47` every 4 minutes | This looks like a possible beacon pattern because it happens regularly, and the address matches Week 7. However, the timing alone does not prove the purpose of the connections. |
| Dot-prefixed file | `/tmp/.mhs_session` | Since the name starts with a dot, it is hidden from normal directory listings. We should check what is in it before making assumptions. |

---

## Dynamic Analysis Assessment

I think running `mhs_monitor` on a normal test server with internet access would be risky because we do not really know what it might do. It could connect to outside servers, download something else, or let someone know the program is being investigated. I would recommend an isolated sandbox with monitoring tools, controlled or fake network connections, and snapshots so it can be reset if needed. The authorized incident response or malware analysis team should handle that testing instead of running the program on the production server.

---

## Escalation Summary

I think this incident should be sent to the incident response team because several findings seem suspicious. The strongest signs are the repeated connections to `185.220.101.47`, the fact that this is the same address from Week 7, and the unknown program running on an EHR server without being listed as approved software. The stripped executable, hidden file path, browser-like user-agent, and `libcurl.so.4` dependency also give the team more things to check. Since the server handles electronic health records, there could be a risk to patient information, but we do not have enough evidence to say any data was accessed or stolen.

Before anyone changes or stops the program, I think the team should preserve the relevant logs, network connection records, process information, a copy and hash of the binary, and information about the temporary file. The incident response team should decide if the server needs to be isolated or if the process should be stopped. As a Tier 1 analyst, my job is to document the evidence and report my concerns, not make production changes on my own.

---

*CPSC 4584 | Governors State University | Fall 2026*
