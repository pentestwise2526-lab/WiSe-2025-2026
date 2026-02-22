CVE ID: EDU-Hydra_Nexus-2026-T01-004

Title: Insecure Direct Object Reference in Hydra-Nexus User Details Endpoint

🔹 Affected Lab and Component

Lab Name: hydra-nexus

Component: User profile information

Endpoint: /user/details

Parameter: id

Container: websploit-hydra-nexus

Port: 5010

🔹 Vulnerability Classification

CWE: CWE‑639 – Authorization Bypass Through User‑Controlled Key

OWASP Top 10: A01:2021 – Broken Access Control

🔹 CVSS v3.1 Score

Score: 7.5 (High)

Vector:

CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N

🔹 Description

The Hydra‑Nexus application fails to enforce object‑level authorization when
handling requests for user profile information. The user identifier parameter
is directly trusted without validating ownership.

An authenticated attacker can modify the identifier value to access sensitive
details of other users, including administrative accounts.

🔹 Exploitation Steps

Access the Hydra‑Nexus application.

Authenticate using a normal user account.

Navigate to the user details functionality.

Observe a request containing a user identifier parameter.

Modify the identifier value to reference another user.

Observe that unauthorized user details are disclosed.

🔹 Proof of Concept (PoC)
GET /user/details?id=1


Result: Sensitive profile data of another user is returned.

🔹 Impact

Successful exploitation may allow:

Disclosure of sensitive user information

Enumeration of privileged accounts

Facilitation of further privilege escalation attacks

🔹 Remediation

Enforce object‑level authorization on all user‑specific endpoints

Validate that the requesting user owns the requested object

Avoid exposing direct object identifiers where possible

Discovered By Team 1
 
Date : 31/01/2026
