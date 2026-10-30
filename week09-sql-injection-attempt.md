# Week 9: SQL Injection Attempt on Appointment Scheduling System

**Course:** CPSC 4584 | Special Topics in Information Security  
**Date:** October 30, 2026  
**Analyst:** Pierre Parker  
**Case ID:** SQLI-2026-1024-001

---

## Incident Summary

On October 24, 2026, between 2:14 AM and 3:01 AM, there were 6 search entries from one IP address in the Maplewood appointment scheduling system. It started off looking like a normal search, but the searches got more suspicious and eventually tried to pull patient names, birthdays, and Social Security numbers. We don't know yet if any patient information was actually returned.

---

## SQL Injection Log Analysis

| Entry | Time | Decoded Input | Type | Severity |
|-------|------|---------------|------|----------|
| 1 | 02:14:07 | `Johnson` | Benign Input | Info |
| 2 | 02:14:52 | `'` | Quote Probe | Medium |
| 3 | 02:18:31 | `Smith' --` | Comment Syntax Test | High |
| 4 | 02:23:14 | `' OR '1'='1` | Tautology Pattern | High |
| 5 | 02:48:39 | `' UNION SELECT NULL,NULL,NULL --` | UNION Injection Attempt | Critical |
| 6 | 03:01:22 | `' UNION SELECT patient_name,date_of_birth,ssn FROM patients --` | Targeted PHI Extraction Attempt | Critical |

---

## Attack Progression Summary

**Entry 1:** This one was just a normal search for Johnson. It kind of gives us a starting point before the searches got suspicious.

**Entry 2:** Here they entered a single quote. I think they were checking if it would mess up the SQL query or cause an error.

**Entry 3:** They searched for `Smith' --`, which uses comment symbols. That looks like an attempt to make the database ignore part of the original query.

**Entry 4:** They tried `' OR '1'='1`, and the `1=1` part is always true. So basically, they may have been trying to get the system to show more results than it should.

**Entry 5:** They used `UNION SELECT` with three `NULL` values. This looks like they were testing whether they could add their own results to the search, and possibly checking how many columns the original query has.

**Entry 6:** This was the most concerning entry because it asked for `patient_name`, `date_of_birth`, and `ssn` from the `patients` table. It could mean the attacker knew something about the database, but they also could have been guessing common names. The log shows the attempt, not proof that any patient information was actually returned.

---

## Remediation Summary

**Primary application control:** I think the biggest fix is using prepared statements with bound parameters instead of putting the user's search text directly into SQL. That way, even if somebody types in SQL injection stuff, the database treats it like regular search text instead of running it as code.

**Input handling:** The application should also check that the search input makes sense for a last name, like checking the length and expected characters. But just blocking quotes isn't enough because attackers could try other things. The parameterized query is still the main fix.

**Database permissions:** The database account should only be allowed to access what the appointment search actually needs. Right now, the account has `SELECT` permission on both the appointments and patients tables, which could make the problem worse if an injection works.

**Detection and monitoring:** I would also recommend better application and database logging, alerts for weird search patterns like `UNION SELECT`, and a WAF to help catch suspicious requests. The WAF would be an extra layer, not a replacement for fixing the code.

---

## Escalation Summary

I think this should be sent to the supervisor and the application, DBA, privacy, and legal teams because it's a serious risk. The six log entries show that somebody tried different SQL injection patterns, and the last one specifically went after patient information. We also know the PHP search puts user input straight into a SQL query without parameterization, and there's no WAF. But the access logs don't tell us if the database actually returned patient data. The application team should work on fixing the search query, and the DBA team should check database query logs and application logs to see what really happened. We should also save the logs and other evidence. Once we know whether anything was returned or accessed, the privacy and legal teams can figure out whether this counts as a data disclosure and if notifications are needed. Until then, I wouldn't say patient data was stolen because we don't have proof of that.

---

*CPSC 4584 | Governors State University | Fall 2026*
