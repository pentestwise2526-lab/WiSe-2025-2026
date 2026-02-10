 CVE ID: EDU-phantom_script-2026-T01-001
 
Title: Reflected Cross‑Site Scripting (XSS) in Phantom‑Script Profile Endpoint

 Affected Lab and Component
* *Lab Name:* phantom-script
* *Component:* Profile page
* *Endpoint:* /profile
* *Parameter:* bio
* *Container:* websploit-phantom-script
* *Port:* 5006

 Vulnerability Classification
* *CWE:* CWE‑79 – Improper Neutralization of Input During Web Page Generation (Cross‑site Scripting)
* *OWASP Top 10:* A03:2021 – Injection

🔹 CVSS v3.1 Score
* *Score:* *6.1 (Medium)*
* *Vector:*

CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N
🔹 Description

The Phantom‑Script application reflects user‑supplied input from the bio parameter directly into the HTML response of the /profile endpoint without proper output encoding or sanitization. This behavior allows an attacker to inject arbitrary JavaScript code, which is executed in the victim’s browser context when the crafted URL is visited.

Because the vulnerability is reflected, successful exploitation requires user interaction, such as clicking a malicious link. If exploited, attackers may perform client‑side attacks including session theft, credential harvesting, or malicious redirection.

🔹 Exploitation Steps

1. Access the profile endpoint:
   http://<host>:5006/profile?user=guest&bio=test
2. Replace the bio parameter value with a JavaScript payload: 
   <script>alert(document.domain)</script>  
3. Load the crafted URL in a browser.
4. Observe JavaScript execution, confirming reflected XSS.

### 🔹 Proof of Concept (PoC)
GET /profile?user=guest&bio=<script>alert(document.domain)</script>

🔹 Impact
Successful exploitation may allow:

* Execution of arbitrary JavaScript in the victim’s browser
* Theft of session cookies (if not HttpOnly)
* Phishing or UI redressing attacks
* Client‑side malware delivery

🔹 Remediation

* Implement proper output encoding for all user‑supplied data
* Enable auto‑escaping in the templating engine
* Apply Content Security Policy (CSP)
* Validate and sanitize input server‑side

Discovered By Team 1 

Date : 30/01/2026
