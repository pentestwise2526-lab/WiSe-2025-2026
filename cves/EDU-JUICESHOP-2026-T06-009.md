CVE-ID: EDU-JUICESHOP-2026-T06-009

Title: Authentication Bypass via SQL Injection in Login Functionality Leading to Unauthorized Admin Access

Affected Application: OWASP Juice Shop
Component: Login Authentication Mechanism
Severity: Critical

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N
CVSS Score: 9.1

Description:

The login functionality of OWASP Juice Shop is vulnerable to SQL Injection, allowing attackers to bypass authentication controls and gain unauthorized access to privileged accounts.

The application improperly incorporates user-supplied input into backend SQL queries without proper sanitization or parameterization. By submitting a crafted SQL payload during authentication, an attacker can manipulate the query logic to always evaluate as true, effectively bypassing the login verification process.

Using the payload ' OR TRUE--, an attacker can successfully authenticate without valid credentials and obtain access to an administrative account. After authentication bypass, the attacker can directly navigate to the administrative interface (/#/administration) and access restricted administrative functionality.

This vulnerability demonstrates a failure to implement secure input handling and proper query parameterization, which falls under the Injection category of the OWASP Top 10 (2021).

Proof of Concept:

Attack Method:
The attacker injects a malicious SQL payload into the login form to manipulate backend authentication queries.

Result:
Authentication is bypassed, allowing access to the administrative section of the application without valid credentials.

Steps to Reproduce:

Navigate to the login page of OWASP Juice Shop.

In the login form, enter the following SQL injection payload: ' OR TRUE--

Submit the login request.

After successful authentication bypass, use the browser address bar to navigate to: /#/administration

Observe that the administrative section of the store is accessible, confirming unauthorized administrative access.

Remediation:

Use Parameterized Queries: All database queries should use prepared statements to prevent user input from altering query logic.

Input Validation and Sanitization: Implement strict validation of authentication inputs and reject suspicious characters commonly used in SQL injection attacks.

Secure Authentication Logic: Ensure that authentication queries cannot be manipulated by user input.

Web Application Firewall (WAF): Deploy WAF rules capable of detecting and blocking SQL injection attempts.

Regular Security Testing: Perform continuous security testing aligned with best practices recommended by the OWASP to detect injection vulnerabilities early.

Discovered By: Team 6
Date: 10 March 2026
