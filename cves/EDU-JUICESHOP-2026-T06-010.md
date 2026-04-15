CVE-ID: EDU-JUICESHOP-2026-T06-010

Title: Deleting Feedback Using Unauthorized Administrative Actions in OWASP Juice Shop

Affected Application: OWASP Juice Shop
Component: Login Authentication Mechanism

Severity: Critical

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N
CVSS Score: 9.1

Description:

The login functionality of OWASP Juice Shop is vulnerable to SQL Injection, allowing attackers to bypass authentication and gain unauthorized access to administrative privileges.

The application fails to properly sanitize or parameterize user-supplied input before incorporating it into backend SQL queries. By injecting a crafted SQL payload during authentication, an attacker can manipulate the query logic so that the authentication condition always evaluates to true.

Using the payload 'OR TRUE--, an attacker can bypass the login verification process and gain access to an administrative account without valid credentials. Once authenticated, the attacker can access the administrative interface and perform privileged actions such as managing or deleting customer feedback.

This vulnerability demonstrates improper input validation and insecure query construction, which falls under the Injection category of the OWASP Top 10 (2021).

Proof of Concept:

Attack Method:

An attacker injects a malicious SQL payload into the login form to bypass authentication and obtain administrative access.

Result:

Authentication is bypassed, allowing unauthorized access to the administrative interface where privileged operations can be performed, including deleting customer feedback entries.

Steps to Reproduce:

Open OWASP Juice Shop and navigate to the Login page.

In the login form, enter the following SQL injection payload in the login field: 'OR TRUE--

Submit the login request to bypass authentication and gain access to the application.

Use the search bar and type administrator to navigate to the Admin page.

In the Administration page, locate and delete the 5-star customer feedback entries.

The application confirms that all 5-star feedback has been removed, demonstrating unauthorized administrative access.

Remediation:

Use Parameterized Queries: Implement prepared statements to ensure user input cannot alter SQL query logic.

Input Validation and Sanitization: Validate authentication inputs and reject characters commonly used in SQL injection attacks.

Secure Authentication Logic: Ensure login queries cannot be manipulated by user-supplied input.

Web Application Firewall (WAF): Deploy a WAF capable of detecting and blocking SQL injection attempts.

Regular Security Testing: Conduct periodic security assessments following OWASP testing guidelines to identify injection vulnerabilities early.

Discovered By: Team 6
Date: 10 March 2026
