**CVE‑ID**: EDU‑JUICESHOP‑2026‑T03‑015

**Title**: Authentication Bypass via SQL Injection in Login Bender Challenge

**Affected Lab**: Juice Shop

**Component**: Authentication/Login Functionality

**Severity**: High

**CVSS Vector**: AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N

**CVSS Score**: 8.1

**Description**:
The Juice Shop login functionality is vulnerable to a SQL Injection authentication bypass that allows an attacker to log in as arbitrary users without knowing their passwords. Specifically, in the Login Bender challenge, the application fails to properly sanitize user‑controlled input submitted through the email field. This can be abused by injecting SQL comment sequences to terminate the password check and force the query to return the specified user record (e.g., bender@juice‑sh.op) without validating the password.
This vulnerability stems from interpolating user input directly into a database query without the use of parameterized statements or prepared queries. As a result, injection payloads such as an email followed by a SQL comment can alter the intended logic of the authentication query.

**Proof of Concept**:

<img width="2810" height="1655" alt="image" src="https://github.com/user-attachments/assets/de025522-513d-4ee5-8d7a-1f941952009f" />

Payload:
bender@juice‑sh.op'-


**Steps to Reproduce**:
1. Open the Juice Shop application and navigate to the login page.
2. In the Email field, input: bender@juice‑sh.op'-
3. Enter an arbitrary string in the Password field (it will be ignored due to the SQL comment).
4. Submit the login form.
5. Observe that the application logs you in as the user “Bender” without requiring a valid password.

**Remediation**:
1. Use parameterized queries / prepared statements for all database interactions.
2. Strictly validate and sanitize all user input before incorporating into DB queries.
3. Employ ORM frameworks that abstract raw SQL when possible.
4. Implement web application firewalls (WAF) against injection patterns.
5. Add server‑side input constraints and reject suspicious strings (e.g., quotes followed by SQL comments).

**Discovered By**: Team 3

**Date**: 2026‑03‑31
