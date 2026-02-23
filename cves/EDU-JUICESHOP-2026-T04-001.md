**CVE-ID**: EDU-JUICESHOP-2026-T04-001
**Title**:  Security Misconfiguration: Permissive CORS Policy
**Affected Lab**:  Juice Shop (`10.6.6.12`)
**Component**:  Global HTTP Headers
**Severity**:  Medium
**CVSS Vector**:  `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N`
**CVSS Score**:  5.3
**Description**:  The application's web server is configured with a wildcard (`*`) for the `Access-Control-Allow-Origin` header. This configuration allows any third-party domain to perform cross-origin requests and read sensitive response data from the application, potentially leading to data theft if a logged-in user visits a malicious website.

**Proof of Concept**:

<img width="940" height="313" alt="Image" src="https://github.com/user-attachments/assets/b30ae23b-48f1-4800-bebe-7812d8b2add0" />

Payload:
``fetch('http://10.6.6.12:3000/api/Challenges').then(res => res.json()).then(console.log);``

**Steps to Reproduce**:
1. Perform a banner grab or Nmap scan on the target `10.6.6.12:3000`
2. Observe the` Access-Control-Allow-Origin: *` header in the response.
3. Use a browser console to execute a `fetch()` request to the API from a different origin (like the PoC with the payload).
4. Observe that the browser allows the data to be read.

**Remediation**:
1. **Implement an Allow-list:** Replace the wildcard (*) with a specific list of trusted domains.
2. **Restrict Credentials**: Never allow `Access-Control-Allow-Credentials`: true when using a wildcard origin.
3. **Short-lived Preflights**: Set a reasonable` Access-Control-Max-Age` to ensure the browser re-verifies the policy frequently.

Useful links:
- [CORS](https://portswigger.net/web-security/cors)
- [CORS Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

**Discovered By**:  Team 4
**Date**:  10/02/2026
