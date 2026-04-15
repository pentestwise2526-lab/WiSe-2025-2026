CVE-ID: EDU-JUICESHOP-2026-T06-006

Title: Privilege Escalation via Role Parameter Manipulation in User Registration API

Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: User Registration API (/api/Users)
Severity: Critical

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N
CVSS Score: 9.1

Description:

The user registration functionality of the OWASP Juice Shop application is vulnerable to privilege escalation due to improper server-side authorization controls. The backend API accepts a user-supplied role attribute during account creation without validating or restricting privilege assignment.

An unauthenticated attacker can intercept the registration request and modify the JSON payload to include elevated privileges by setting the role value to "admin". Because the server trusts client-provided input, the application creates a new account with administrator permissions.

This vulnerability allows attackers to gain unauthorized administrative access, enabling full control over privileged application functionality, including management operations and access to sensitive data.

Proof of Concept:

Intercepted Registration Request:
A normal registration request is captured using an intercepting proxy.

Malicious Payload Modification:
The attacker modifies the JSON body by adding:
"role":"admin"

Result:

The application successfully creates a user account with administrator privileges.
Upon login, the account has elevated permissions, and the challenge “Admin Registration” is marked as solved.

Steps to Reproduce:

Navigate to the registration page of OWASP Juice Shop.

Open Burp Suite and enable request interception.

Attempt to create a new account and capture the registration request.

Send the intercepted request to Repeater.

Modify the JSON request body by adding the following parameter:

"role":"admin"

Forward the modified request to the server.
7 . Log in using the newly created account.

8 . Observe that the account possesses administrator privileges, confirming successful privilege escalation.

Remediation:

Server-Side Authorization Enforcement: Ignore client-supplied privilege fields during registration and assign default roles server-side.

Role Whitelisting: Explicitly restrict allowed roles for public registration (e.g., customer only).

Input Sanitization & Validation: Validate request schema and reject unauthorized attributes.

Secure API Design: Separate privilege management endpoints from public registration functionality.

Principle of Least Privilege: Ensure newly registered users receive minimal permissions by default.

Discovered By: Team 6
Date: 2 March 2026
