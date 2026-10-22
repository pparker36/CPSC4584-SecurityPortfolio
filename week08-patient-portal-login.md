# Week 8: Patient Portal Login Page Behaving Strangely
**Course:** CPSC 4584 | Special Topics in Information Security  
**Date:** October 19, 2026  
**Analyst:** Pierre Parker  
**Case ID:** WEB-2026-1019-001

---

## Incident Summary

On October 17, three patients reported strange behavior when logging into the Maplewood patient portal. After reviewing the HTTP traffic and server logs, the SOC found four security issues in the original application setup, but there is no confirmed evidence that someone exploited them.

---

## HTTP Vulnerability Findings

| Finding | Location | Observed Value | Vulnerability | Severity |
|---------|----------|----------------|---------------|----------|
| 1 | Login request method | `GET /login?username=jsmith&password=Summer2026!` | Login credentials exposed in the URL | High |
| 2 | Login response `Set-Cookie` header | `Set-Cookie: sessionid=a3f8c2d1; Path=/` (missing `HttpOnly` and `Secure`) | Weak session cookie protection | High |
| 3 | Login response headers | Missing `Strict-Transport-Security`, `Content-Security-Policy`, and `X-Frame-Options` | Missing security response headers | Medium |
| 4 | Post-login redirect | `next=https://external-site.com/collect` was accepted and later used as a redirect destination | Unvalidated redirect | High |

---

## Analysis Summary

**Finding 1:** The login used a GET request that put the username and password right in the URL. That's a problem because URLs can get saved in browser history, server logs, or proxy logs. The SOC already confirmed that login URLs containing credentials appeared in the application server logs. I would recommend changing the login to use POST over HTTPS so the credentials are sent in the request body instead of the URL. HTTPS encrypts traffic in transit, but it doesn't make passwords safe if they're already placed in URLs.

**Finding 2:** The session cookie in the login response was missing both `HttpOnly` and `Secure`. Without `HttpOnly`, JavaScript could read the cookie, which increases the risk of session theft if an XSS issue exists. Without `Secure`, a browser could send the cookie over an unencrypted HTTP connection. I would recommend adding both attributes and reviewing `SameSite`, session handling, and XSS protection too. Those settings handle different risks, so one doesn't replace the others.

**Finding 3:** The login response didn't include `Strict-Transport-Security`, `Content-Security-Policy`, or `X-Frame-Options`. These missing headers can weaken HTTPS enforcement and protection against certain script or framing attacks. I would recommend that the development and security teams review the application and add appropriate headers with settings that work for the portal.

**Finding 4:** The portal accepted an outside link through the `next` redirect parameter and later sent users there. That could let someone make a real portal link redirect patients to a fake website. I would recommend validating redirect destinations and only allowing approved internal paths or trusted destinations.

---

## HTTP Header Inspection Commands Used

These commands were run against **`example.com` as an approved practice site**, not the Maplewood production portal. The results are separate from the four Maplewood findings above.

| Command | Purpose and what I observed |
|---------|-----------------------------|
| `curl -I https://example.com 2>/dev/null` | Checked the response headers. I saw `HTTP/2 200` and `content-type: text/html; charset=utf-8`. There was no `Set-Cookie` header in this response. |
| `curl -I https://example.com 2>/dev/null \| grep -iE "strict\|content-security\|x-frame\|x-content\|referrer"` | Looked for common security headers. The filter returned no matching lines in the response I checked. That alone doesn't prove the practice website is vulnerable. |
| `curl -v --max-time 5 https://example.com 2>&1 \| grep -E "^[<>]" \| head -20` | Looked at the HTTP request and response. Lines starting with `>` showed the request, including `GET / HTTP/2` and `Host: example.com`. Lines starting with `<` showed the response, including `HTTP/2 200` and the content type. This is similar to what I'd inspect in a browser's Network tab. |
| `curl -IL --max-time 5 https://example.com 2>/dev/null \| grep -E "HTTP\|Location\|Set-Cookie"` | Checked for redirects and cookies. The output showed `HTTP/2 200` with no redirect responses, `Location` headers, or `Set-Cookie` headers. |

---

## Escalation Summary

After going through the evidence, I found four issues that should be sent to the Maplewood application and security teams. The login URL included a username and password, the session cookie was missing `HttpOnly` and `Secure`, the login response didn't have several security headers, and the redirect feature accepted an outside website. These problems could expose login information, put sessions at risk, or send patients to unsafe websites. The credential-bearing URLs in the reviewed server logs are confirmed, but active exploitation hasn't been confirmed.

My suggested next steps are to switch login requests to POST over HTTPS, protect session cookies, review and add the missing security headers, and limit redirects to approved destinations. **These are recommendations, not fixes I've completed.** As a Tier 1 analyst, my job is to document what the evidence shows and escalate it to my supervisor and the right teams. Authorized developers and security engineers would decide on the changes, review any additional logs with permission, test the fixes, and handle deployment. I didn't change or test the production portal.

---
*CPSC 4584 | Governors State University | Fall 2026*
