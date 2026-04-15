CVE-ID: EDU-JUICESHOP-2026-T06-015

Title: SQL Injection Allowing Authentication with Deleted User Accounts (GDPR Data Erasure Bypass)

Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: Authentication API (/rest/user/login)
Severity: High

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N
CVSS Score: 8.2

Description:

The authentication mechanism in the OWASP Juice Shop application is vulnerable to SQL Injection, allowing attackers to bypass logical controls intended to prevent access to deleted user accounts.

According to GDPR principles, when a user account is erased, it should no longer be accessible for authentication or processing. However, due to insufficient sanitization of user input in the login functionality, attackers can manipulate the SQL query used to validate user credentials.

By injecting a crafted SQL condition into the email field, an attacker can alter the query logic to target accounts that have been marked as deleted (i.e., where the deletedAt field is not null). This effectively bypasses the intended restriction and allows authentication with accounts that were previously erased.

This vulnerability demonstrates non-compliance with data erasure requirements, as deleted accounts can still be accessed through manipulated authentication queries.

Proof of Concept:

Target Endpoint:
POST /rest/user/login

Example payload used during testing:

Email Field:

' OR deletedAt IS NOT NULL --

Password Field:

anypassword

Result:

The manipulated query causes the authentication logic to match a user record where the deletedAt field contains a value, which represents a previously deleted account.

The application successfully logs in using this erased account, confirming that deleted user data can still be accessed and processed. The “GDPR Data Erasure” challenge is marked as solved, confirming successful exploitation.

Steps to Reproduce:

Navigate to the OWASP Juice Shop login page.

In the Email field, enter the following SQL injection payload:
' OR deletedAt IS NOT NULL --

In the Password field, enter any arbitrary password.

Submit the login request.

Observe that the login is successful despite the account being marked as deleted.

The challenge “GDPR Data Erasure (Log in with Chris' erased user account)” is marked as solved, confirming successful exploitation.

Remediation:

Input Sanitization: Implement strict validation and sanitization of all user input fields to prevent SQL injection attacks.

Parameterized Queries: Use prepared statements or parameterized queries to ensure user input cannot modify SQL query logic.

Secure Authentication Logic: Ensure that login queries explicitly enforce conditions preventing authentication of deleted or disabled accounts.

Database Constraints: Implement database-level safeguards to prevent processing of records marked as deleted.

Security Testing: Conduct regular penetration testing and code reviews to detect injection vulnerabilities in authentication mechanisms.

Discovered By: Team 6

Date: 31 March 2026
