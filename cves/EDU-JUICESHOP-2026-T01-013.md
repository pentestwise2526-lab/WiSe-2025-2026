EDU-JUICESHOP-2026-T01-013

Title:
Unauthenticated CAPTCHA Bypass via Missing Server-Side Rate Limiting in Customer Feedback Submission

Affected Lab and Component

Lab: OWASP Juice Shop

 Component: Customer Feedback API
 
 Affected Endpoint:
POST /api/Feedbacks

Port: 3000

Vulnerability Classification: 
CWE: CWE-307 – Improper Restriction of Excessive Requests


OWASP Top 10: A07:2021 – Identification and Authentication Failures


Vulnerability Type: Business Logic Flaw / Security Misconfiguration



CVSS v3.1 Score and Vector


CVSS Score: 7.1 (High)

Vector:

CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:L/A:L


Description
The OWASP Juice Shop customer feedback functionality includes a CAPTCHA mechanism intended to prevent automated submissions. However, the backend feedback API does not enforce CAPTCHA validation or rate limiting on the server side.

An unauthenticated attacker can submit multiple feedback requests in rapid succession using Burp Suite Intruder without logging in. Even though CAPTCHA parameters (captchaId and captcha) are present in the request, the server does not validate their correctness or enforce submission limits. This allows an attacker to submit more than 10 feedback entries within 10 seconds, bypassing the CAPTCHA protection entirely.

Exploitation Steps

Step 1: Access Customer Feedback Anonymously
Open OWASP Juice Shop in a browser.


Do not log in.


Navigate to the Customer Feedback page.


Submit a single feedback entry to observe normal behavior.



Step 2: Intercept Feedback Submission Request
Launch Burp Suite.


Enable Proxy → Intercept.


Submit feedback from the browser.


Capture the following request:


POST /api/Feedbacks HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{
  "comment": "Nice application",
  "rating": 5,
  "captchaId": 7,
  "captcha": "3"
}


Step 3: Send Request to Burp Intruder
Right-click the captured request.


Select Send to Intruder.


Navigate to Intruder → Positions.


No changes were made to the Positions tab, as modifying positions caused request errors.


CAPTCHA parameters remain included in the request.



Step 4: Configure Intruder Payloads
Go to Intruder → Payloads.


Set:


Payload type: Null payloads


Payload count: 10


This causes the same feedback submission request to be replayed multiple times in quick succession.

Step 5: Execute the Attack
Click Start attack in Burp Intruder.


Observe multiple responses with status code:


201 Created

More than 10 feedback submissions are accepted within 10 seconds, despite CAPTCHA being present.

Proof of Concept
By replaying the same CAPTCHA-protected feedback submission request multiple times using Burp Suite Intruder, an unauthenticated attacker can bypass CAPTCHA protections and successfully flood the feedback system.

Impact

CAPTCHA protection rendered ineffective


Unauthenticated feedback spam possible


Automated abuse of public-facing functionality


Potential denial-of-service against feedback processing


Degradation of application trust and data integrity



Remediation

Enforce CAPTCHA validation on the server side


Bind CAPTCHA to:


Client session or IP


Single-use tokens


Expiration timestamps


Implement strict rate limiting on /api/Feedbacks


Reject excessive anonymous submissions


Monitor and alert on abnormal feedback submission rates


Discovered By: Team 1

Date: 10/02/2026
