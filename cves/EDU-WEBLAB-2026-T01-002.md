🔹 CVE ID: EDU-WEBLAB-2026-T01-002
🔹 Title : OS Command Injection in Shell‑Inject Ping Function

🔹 Affected Lab and Component
* *Lab Name:* shell-inject
* *Component:* Ping functionality
* *Input Field:* Hostname / target input
* *Container:* websploit-shell-inject
* *Port:* 5002

🔹 Vulnerability Classification

* *CWE:* CWE‑78 – Improper Neutralization of Special Elements used in an OS Command
* *OWASP Top 10:* A03:2021 – Injection


 🔹 CVSS v3.1 Score

* *Score:* *9.8 (Critical)*
* *Vector:*


CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H

🔹 Description

The Shell‑Inject application executes operating system commands by directly concatenating user‑supplied input into a shell command without validation or sanitization. This allows an attacker to inject arbitrary OS commands using shell metacharacters.

The vulnerability does not require authentication or user interaction and can be exploited remotely, resulting in full compromise of the underlying system.

 🔹 Exploitation Steps

1. Access the ping functionality of the application.
2. Submit a legitimate hostname (e.g., google.com) to observe normal behavior.
3. Append a shell command using metacharacters:
   google.com; whoami
   4. Observe execution of the injected command in the response output.

🔹 Proof of Concept (PoC)
google.com; id

🔹 Impact
Successful exploitation may allow:

* Remote Command Execution (RCE)
* Full system compromise
* Unauthorized access to sensitive files
* Service disruption or malware installation

 🔹 Remediation

* Avoid using shell execution functions with user input
* Use parameterized system calls or safe APIs
* Apply strict input validation and allowlists
* Run services with least privilege


Discovered By Team 1
 
Date : 30/01/2026

