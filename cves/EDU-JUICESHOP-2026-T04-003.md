**CVE-ID**: EDU-JUICESHOP-2026-T04-003
**Title**:  Authentication Bypass via SQL Injection
**Affected Lab**:  Juice Shop (`10.6.6.12`)
**Component**:   `/#/login`
**Severity**:  Critical
**CVSS Vector**:  `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`
**CVSS Score**:  9.8
**Description**:  The login form is vulnerable to classic SQL injection. An attacker can bypass authentication by injecting SQL syntax into the email field, granting full administrative access without a valid password.

**Proof of Concept**:

<img width="940" height="595" alt="Image" src="https://github.com/user-attachments/assets/ea46dc18-4a3a-45ef-89aa-5b1e5c843132" />

<img width="940" height="324" alt="Image" src="https://github.com/user-attachments/assets/1aac1b3c-3e32-409a-8761-bea749d5ec0d" />

Payload:
`' or 1=1 --` to the email textbox.

**Steps to Reproduce**:
1. Navigate to `http://10.6.6.12:3000/#/login`
2. Enter `' or 1=1 --` in the `email` field and any text in the `password` field.
3. Enter anything in the `password` field.
4. Click "Log In."

**Remediation**:
Use Prepared Statements (Parameterized Queries) and input sanitization to ensure user input is never executed as code. [Guide](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

**Discovered By**:  Team 4
**Date**:  22/02/2026
