🔹 CVE ID : EDU-Juice-Shop-2026-T01-007

🔹 Title: JWT Role Manipulation Leading to Privilege Escalation

🔹 Affected Lab and Component:

Lab Name: OWASP Juice Shop

Component: Authentication & Authorization

Token Type: JSON Web Token (JWT)

Storage: Browser Local Storage

Container: bkimminich/juice-shop

Port: 3000

🔹 Vulnerability Classification:


CWE: CWE‑287 – Improper Authentication

OWASP Top 10: A02:2021 – Broken Authentication

🔹 CVSS v3.1 Score:

Score: 8.1 (High)
Vector:
CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:N

🔹 Description:


The Juice Shop application relies on JWTs to store and validate user authentication and authorization information. Sensitive claims such as the user role are stored client‑side within the token and trusted without sufficient server‑side verification.

An attacker can modify the role claim inside the JWT from customer to admin and reuse the token, resulting in unauthorized administrative access.

🔹 Exploitation Steps:


Authenticate as a normal user.
Extract the JWT from browser Local Storage.
Decode the token payload using base64 decoding.
Modify the role claim from customer to admin.
Re‑encode the payload and reconstruct the JWT.
Replace the token in Local Storage.
Refresh the application and access admin‑only endpoints.

🔹 Proof of Concept (PoC):


Modified JWT payload:
{
  "id": 23,
  "email": "vidhate@gmail.com",
  "role": "admin",
  "iat": 1769936438
}

Admin endpoint accessed:
GET /#/administration

🔹 Impact:


Successful exploitation may allow:
Unauthorized administrative access
Full control over application configuration
User data exposure or manipulation
Severe business logic compromise

🔹 Remediation:

Do not trust client‑side JWT claims for authorization
Validate user roles server‑side on each request
Use short‑lived access tokens
Implement role‑based access control (RBAC)
Rotate and protect JWT signing keys
Discovered By Team 1
 
Date : 1/02/2026

