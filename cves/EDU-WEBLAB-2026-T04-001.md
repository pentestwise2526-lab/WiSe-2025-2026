**CVE-ID**: EDU-WEBLAB-2026-T04-001  
**Title**: Insecure Session Management (Cookie Flags)
**Affected Lab**: WebGoat (10.6.6.11)
**Component**:  `/WebGoat`
**Severity**:  High
**CVSS Vector**:  CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N
**CVSS Score**:  7.5
**Description**:  The application issues a `JSESSIONID` cookie without the `Secure` or `HttpOnly` attributes. The absence of Secure allows the cookie to be sent over unencrypted HTTP. The absence of `HttpOnly` allows the cookie to be accessed by client-side scripts (e.g., JavaScript), making the session vulnerable to theft via XSS.

**Proof of Concept**:
<img width="778" height="255" alt="Image" src="https://github.com/user-attachments/assets/9784f8d5-8d02-47b8-9bb6-028a586d5177" />

_Note the missing `Secure` and `HttpOnly` attributes in the `Set-Cookie`_

**Steps to Reproduce**:
1. Open Burp Suite or other traffic interceptor and ensure the interceptor is configured for your browser.
2. Enter the URL for the WebGoat login page: `http://<webgoat IP>/WebGoat/login`.
3. Perform a login attempt (even a failed one will work) and locate the server's response in the `Proxy` > `HTTP` History tab.
4. Click on the response and search for the `Set-Cookie` header.
5. Observe that the header contains `path=/WebGoat` but is missing the `; Secure` and `; HttpOnly` attributes.
6. To confirm further, open the browser's `Developer Tools` (F12) > `Console`, and type `alert(document.cookie);`. If an alert box appears showing the `JSESSIONID`, you have confirmed the lack of `HttpOnly`.

**Remediation**:
1. Regenerate JSESSIONID on every authentication. [Example](https://stackoverflow.com/a/8165963)
2. Add `Secure` and `HttpOnly` flag in the `Set-Cookie` header. [Guide](https://stackoverflow.com/a/34490241)

**Discovered By**:  Team 4
**Date**:  01/02/2026
