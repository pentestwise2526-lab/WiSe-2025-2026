CVE ID: EDU‑hydra‑nexus‑2026‑T01‑006

 Title: Sensitive Information Disclosure in Hydra‑Nexus Admin Control Panel

🔹 Affected Lab and Component

Lab Name: hydra‑nexus

 Component: Admin control panel user information display
 
 Endpoint: /admin/users
 
 Container: websploit‑hydra‑nexus
 
 Port: 5010

🔹 Vulnerability Classification
CWE: CWE‑200 – Exposure of Sensitive Information

 OWASP Top 10: A02:2021 – Cryptographic Failures

🔹 CVSS v3.1 Score

Score: 7.5 (High)

Vector:

CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N

🔹 Description

The Hydra‑Nexus admin control panel exposes sensitive information for both
 administrative and standard user accounts. The interface displays confidential
 fields including password hashes, API keys, and email addresses, which are not
 required for administrative operations.

This excessive data exposure violates secure‑by‑design principles and may allow
 attackers to leverage disclosed secrets for further compromise.

🔹 Exploitation Steps

Authenticate to the Hydra‑Nexus application.

Access the admin control panel.

Navigate to the user management or user details section.

Observe that sensitive information is displayed directly in the interface.


🔹 Proof of Concept (PoC)

Observed Sensitive Fields:

Password hash

API key

Email address

Result:

 Sensitive credential material is disclosed to authenticated users.

🔹 Impact

Successful exploitation may allow:

Disclosure of credential and secret material

Increased risk of account takeover

Lateral movement and privilege escalation

Chaining with other vulnerabilities

🔹 Remediation

Do not display password hashes or API keys in UI responses

Apply strict data minimization and output filtering

Mask or remove sensitive fields from administrative views

Rotate exposed API keys and secrets
 


Discovered By Team 1
 
Date : 1/02/2026
