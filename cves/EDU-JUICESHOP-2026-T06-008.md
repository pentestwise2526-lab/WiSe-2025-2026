CVE-ID: EDU-JUICESHOP-2026-T06-008

Title: Insecure Direct Object Reference (IDOR) via Basket ID Manipulation in Shopping Basket Feature

Affected Lab: OWASP Juice Shop
Component: Basket Management Functionality
Severity: High

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N
CVSS Score: 7.5

Description:
The shopping basket functionality in OWASP Juice Shop is vulnerable to an Insecure Direct Object Reference (IDOR) due to improper access control enforcement on basket identifiers stored in client-side session storage.

The application stores a basket identifier (bid) in the browser’s session storage and relies on this value to retrieve the corresponding shopping basket from the backend. Because the application does not properly validate whether the authenticated user is authorized to access the referenced basket ID, an attacker can modify this identifier to access another user's basket.

By altering the bid value in the browser’s session storage, an authenticated user can view the contents of another user's shopping basket without authorization. This results in an information disclosure vulnerability, exposing sensitive user activity data and demonstrating broken access control.

This vulnerability is a classic example of Broken Access Control as categorized by the OWASP Top 10 (2021).

Proof of Concept:

Attack Method:
The attacker modifies the basket identifier stored in the browser session storage to reference another user's basket.

Result:
The application retrieves and displays another user's shopping basket contents, confirming unauthorized access.

Steps to Reproduce;

Navigate to the OWASP Juice Shop application and log in with a valid user account.

Open the View Basket page (/#/basket).

Open Browser Developer Tools.

Navigate to Application → Session Storage.

Locate the key bid with the value 6.

Modify the value of bid from 6 to 5.

Refresh the page.

Observe that the application loads another user's shopping basket, confirming unauthorized data access.

Remediation:

Server-Side Authorization Checks: Ensure that every request accessing a basket verifies that the authenticated user owns the requested basket ID.

Avoid Client-Side Trust: Do not rely on identifiers stored in client-side storage (sessionStorage/localStorage) as a source of truth for authorization.

Indirect Reference Mapping: Use secure server-side session mappings to associate users with their basket IDs rather than accepting arbitrary identifiers from the client.

Access Control Enforcement: Implement strict access control validation for all object identifiers retrieved from client requests.

Security Testing: Regularly test for IDOR vulnerabilities using methodologies recommended by the OWASP.

Discovered By: Team 6
Date: 8 March 2026
