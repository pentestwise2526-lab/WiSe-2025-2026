**CVE-ID**: EDU-JUICESHOP-2026-T04-005
**Title**:  Improper Input Validation (Negative Star Rating)
**Affected Lab**:  Juice Shop (10.6.6.12)
**Component**:  `/api/Feedbacks`
**Severity**:  Medium
**CVSS Vector**:  `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:L/A:N`
**CVSS Score**:  4.3
**Description**:  The application validates ratings (`1-5`) on the frontend but fails to enforce these limits on the backend API. An attacker can submit arbitrary values (e.g., `-999`) via direct API calls.

**Proof of Concept**:

<img width="940" height="695" alt="Image" src="https://github.com/user-attachments/assets/fa558cc8-5bcb-498f-b1f5-cf3e11dd20d9" />

<img width="940" height="396" alt="Image" src="https://github.com/user-attachments/assets/1c0ec7e7-05c9-414e-b736-f00be9fecb0f" />

Payload:

```
fetch('/api/Feedbacks/', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ comment: "Logic Test", rating: -999, captchaId: 6, captcha: "350" })
}).then(res => res.json()).then(data => console.log(data));
```

**Steps to Reproduce**:
1. Log in to the normal user account and navigate to `http://10.6.6.12:3000/#/contact`
2. Open your Network Tab (F12).
3. Submit a normal feedback but find the `POST` request in the log.
4. Take note of the `captcha` and `captchaId`.
5. Open Console Tab.
6. Execute a fetch `POST` request to `/api/Feedbacks` with a negative rating (use the payload above while replacing the `captcha` and `captchaId`).
7. Log in to the admin account and verify the value in the admin panel's network response.

**Remediation**:
1. Use a library like Joi or `Validator.js` to enforce a strict schema. The rating field should be defined as integer, min: 1, and max: 5.
2. Ensure the backend rejects non-numeric or overflow values (e.g., rating: "five" or rating: 9999999999) which could cause database errors.
3. Configure your API Gateway to drop any requests that do not conform to the expected JSON structure before they even reach your application logic.

[Read More](https://owasp.org/www-community/vulnerabilities/Business_logic_vulnerability)

**Discovered By**:  Team 4
**Date**:  02/03/2026
