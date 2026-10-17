# Week 7: Network Traffic Anomaly During Night Shift

**Course:** CPSC 4584 | Special Topics in Information Security  
**Date:** October 12, 2026  
**Analyst:** Pierre Parker  
**Case ID:** NET-2026-1012-001 (related: FOR-2026-1005-001)

---

## Incident Summary

During Maplewood's night shift, the network logs showed some activity that didn't look normal. Webb's laptop sent data to an unauthorized IP address, and the timing was close to when an insurance report was modified and deleted in the Week 6 disk investigation. There were also some unusual connections from the radiology and billing workstations.

---

## Network Flow Analysis

| Flow | Source | Destination | Classification | Finding |
|------|--------|-------------|----------------|---------|
| 1 | Webb laptop (10.4.2.47) | 185.220.101.47 | Suspicious data transfer | At 11:48 PM on October 3, Webb's laptop sent 2,457,814 bytes over HTTPS. That's close to the size of the deleted insurance report, but we can't tell what was actually sent from the flow logs. |
| 2 | Webb laptop (10.4.2.47) | 185.220.101.47 | Possible beaconing | The laptop connected to the same IP about every four minutes from 11:15 PM to 6:42 AM. Each connection was about 312 bytes. This looks like beaconing, but it doesn't prove malware was involved. |
| 3 | EHR production server (10.4.1.10) | 52.94.228.167 (AWS) | Normal backup traffic | The server transferred 14.2 MB between 2:00 and 2:18 AM. This matched the scheduled nightly EHR backup, so it looks like expected activity. |
| 4 | Radiology workstation (10.4.3.22) | 8.8.8.8 | Unusual DNS activity | The workstation made 23 DNS queries in just eight minutes, from 2:14 to 2:22 AM. That's worth checking, but it doesn't automatically mean there was an attack. |
| 5 | Billing workstation (10.4.3.88) | 203.0.113.99 | Possible beaconing | The billing workstation made nine small HTTP connections to an unapproved IP address about four minutes apart, from 3:47 to 4:19 AM. The pattern is suspicious, but we can't call it command-and-control activity without more proof. |

---

## Disk and Network Evidence Correlation

**Disk (Week 6):** The insurance report on Webb's laptop was about 2.4 MB. It was last modified around 11:47 PM on October 3 and deleted around 11:52 PM.

**Network (Week 7):** Webb's laptop started connecting to 185.220.101.47 at 11:15 PM. At 11:48 PM, it sent 2,457,814 bytes to that same IP address. The smaller connections continued until 6:42 AM the next morning.

**Correlation:** The times and file sizes are pretty close. The larger transfer happened after the report was modified but before it was deleted, so it's possible the file was sent out. That's something I would want the security team to investigate further.

**Limitation:** The flow logs show IP addresses, connection times, and how much data moved, but not the actual contents. I can't say for sure that the insurance report was transferred without more evidence, like packet captures or endpoint logs.

---

## Network Artifact Inspection Commands Used

| Command | Purpose |
|---------|---------|
| `curl -I https://example.com` | I checked the website's response headers and got HTTP/2 200. The headers showed HTML content and Cloudflare as the server, but they didn't tell me what software was behind Cloudflare. |
| `curl -v --max-time 5 https://example.com` | I checked how the connection was set up. It used HTTPS on port 443 and TLS 1.3 with `TLS_AES_256_GCM_SHA384`. This showed more about the connection, but not a full history of network traffic. |
| `curl -s https://example.com -o /tmp/response.html && xxd /tmp/response.html \| head -4` | I downloaded the webpage and checked its first few bytes. It started with `3c21 646f`, which is the beginning of `<!doctype html>`. That matched the HTML content type I expected. |
| `curl -s -o /dev/null -w "HTTP: %{http_code} \| Size: %{size_download} bytes \| Time: %{time_total}s \| IP: %{remote_ip}" https://example.com` | I got HTTP 200, 577 bytes downloaded, a response time of about 0.099 seconds, and IP address 104.20.23.154. The IP address and byte count are similar to details in the Maplewood flow logs, but this command doesn't show repeated traffic over time. |

I used **example.com** for these commands to practice checking network activity. I didn't run these commands against Maplewood's suspicious IP addresses, so the results are just practice evidence, not proof of what happened in the incident.

---

## Escalation Summary

The biggest concern is Webb's laptop because it sent about 2.4 MB to an unauthorized IP address around the same time the insurance report was modified and deleted. It also kept making small connections to that IP address every four minutes, which could be beaconing. The radiology workstation had an unusual number of DNS queries, and the billing workstation made repeated HTTP connections to an unapproved address. The EHR backup, on the other hand, looked like normal scheduled activity.

I also noticed that Webb's destination IP was in the same /24 subnet as an IP from the earlier billing portal attack. That might be worth looking into, but it doesn't mean the same attacker was behind both incidents. I would write down the times, IP addresses, and traffic patterns and share them with my supervisor. I would recommend checking endpoint, DNS, proxy, and packet-capture evidence if it's available. As a junior analyst, I wouldn't block any IP addresses, change firewall settings, contact outside groups, or decide who the attacker was on my own. Those decisions should go through the authorized security team.

---

*CPSC 4584 | Governors State University | Fall 2026*
