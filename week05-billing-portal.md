Week 5: Weak Password Policy Exposes Billing Portal

Course: CPSC 4584 | Special Topics in Information Security

Date: September 28, 2026

Analyst: Pierre Parker

Incident ID: INC-2026-0928-001  

Incident Summary
The credential stuffing attack started when the attacker used a password that had already been leaked in another breach. After many failed attempts, the attacker successfully logged into the billing account and accessed records for 3,247 patients.
TLS Assessment

TLS Version: TLS 1.3
Status: Compliant

What TLS Protected: TLS protected the data being sent between the client and server by encrypting the connection.

What TLS Did Not Protect: TLS could not tell if the person using the valid username and password was actually the real account owner.
Authentication Controls Gap Analysis
Control	Required	Status	Finding

MFA	Maplewood sensitive-account standard	Not Implemented	The account only required a password, so the compromised password was enough for the attacker to log in. MFA would have added another step of verification.
Failed Attempt Protection	Account-based throttling and alerting	Not Implemented	There were 847 failed login attempts from 12 IP addresses, but the system did not slow or block the repeated attempts.
Password Policy	NIST SP 800-63B-4 aligned	Needs Improvement	The password met the local password rules, but it was already known to be compromised. The system did not properly check it against known leaked passwords.
Compromised Credential Response	Detect and invalidate confirmed compromised authenticators	Not Implemented	The reused password was already part of a leaked credential set, but there was no process that forced the password to be changed or invalidated.
Automated Attack Controls	Throttling, bot detection, or adaptive controls as appropriate	Not Implemented	There were no automated controls that properly detected or slowed the unusual login activity before the attacker got access.


OpenSSL Commands Practiced
Command	Purpose
openssl genrsa -out private_key.pem 2048	This command created a 2048-bit RSA private key and saved it in the private_key.pem file.
openssl rsa -in private_key.pem -pubout	This command took the public key information from the private key and displayed or exported the matching public key.
openssl rsa -in private_key.pem -text -noout	This command let me inspect details of the RSA key, such as the modulus and other key information.
cat public_key.pem	This command displayed the public key PEM file, including the BEGIN PUBLIC KEY and END PUBLIC KEY lines and the Base64-encoded key data between them.


Escalation Summary
The billing_admin_03 account was accessed using a compromised password that came from a previous leaked credential set. The attacker stayed logged in for about 47 minutes and viewed records for 3,247 patients. The investigation also showed 847 failed login attempts from 12 different IP addresses before the successful login.
The biggest authentication problems were that MFA was not being used, failed login protection was missing, automated attack controls were not in place, and the system did not properly respond to a known compromised password. TLS 1.3 was working correctly, but it only protected the connection and could not stop someone who had valid login information.
Leadership still needs to determine if any other accounts were affected or if more information was copied. Some possible next steps would be reviewing more logs, checking other accounts for similar activity, and having the security team look into MFA, stronger password screening, and better login monitoring.
CPSC 4584 | Governors State University
