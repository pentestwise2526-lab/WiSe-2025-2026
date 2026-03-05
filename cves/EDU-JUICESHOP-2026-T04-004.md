**CVE-ID**: EDU-JUICESHOP-2026-T04-004
**Title**:  Reflected Cross-Site Scripting (XSS) in Search Function
**Affected Lab**:  Juice Shop (10.6.6.12)
**Component**:  Search Bar Parameter (`q`)
**Severity**:  High
**CVSS Vector**: `CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N`
**CVSS Score**:  6.1
**Description**:  The search functionality fails to sanitize user-supplied input. An attacker can craft a malicious URL containing JavaScript; when a victim clicks it, the script executes in their browser, allowing for session hijacking via `document.cookie`.

**Proof of Concept**:

<img width="940" height="211" alt="Image" src="https://github.com/user-attachments/assets/f5c5b99a-62d3-4eb8-9c6c-9fdb5d61e653" />

<img width="940" height="579" alt="Image" src="https://github.com/user-attachments/assets/1bf3e06f-9fc1-416b-b790-1d6837a100eb" />

Payload:
```
<iframe src="javascript:alert(document.cookie)">
```

**Steps to Reproduce**:
1. Navigate to the homepage.
2. Paste the payload into the search bar.
3. Observe the pop-up containing session data.

**Remediation**:
Implement Output Encoding (convert `<` to `&lt;`, etc.) and a strong Content Security Policy (CSP).

**Discovered By**: Team 4
  
**Date**:  
02/03/2026