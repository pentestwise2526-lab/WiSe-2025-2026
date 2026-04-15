CVE-ID: EDU-JUICESHOP-2026-T06-011

Title: Administrator Account Compromise via Weak Password and Credential Brute Force

Affected Lab: OWASP Juice Shop
Container: websploit-juice-shop | Port: 3000
Component: User Authentication Module (/rest/user/login)
Severity: High

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N
CVSS Score: 8.8

Description:

The authentication mechanism of the OWASP Juice Shop application is vulnerable to credential brute-force attacks due to weak password strength policies and lack of sufficient login rate limiting.

An attacker can intercept the login request and perform automated credential testing using a list of commonly used email addresses and passwords. Because the application does not enforce strong password requirements, account lockout policies, or effective rate limiting, attackers can systematically test credential combinations until valid administrator credentials are discovered.

This vulnerability allows unauthorized attackers to obtain administrative access without exploiting injection vulnerabilities or modifying authentication logic.

Proof of Concept:
Target Endpoint:
POST /rest/user/login

Attack Method:
An attacker uses an intercepting proxy to capture the login request and performs a credential brute-force attack using wordlists of common usernames and passwords.

Result:
The attack eventually identifies valid administrator credentials, allowing successful authentication. The challenge “Password Strength” is marked as solved, confirming unauthorized administrative access.

Steps to Reproduce:

Open Burp Suite and enable request interception.

Navigate to the login page of OWASP Juice Shop.

Attempt to log in using dummy credentials:
username: payload1
password: payload2

4 .In the Proxy tab, capture the POST request sent to the login endpoint.

Send the intercepted request to Burp Intruder.

Configure a Pitchfork attack and mark the username and password fields as payload positions.

Disable payload encoding.

Load runtime wordlists:
Email wordlist for username payloads
Password wordlist for password payloads.

Start the attack.

Monitor the responses and identify a 200 OK response, indicating successful authentication with valid credentials.

Remediation:

Strong Password Policies: Enforce minimum password complexity requirements including length, symbols, and mixed characters.

Account Lockout Mechanisms: Temporarily lock accounts after multiple failed login attempts.

Rate Limiting: Limit the number of login attempts allowed within a defined time period.

Multi-Factor Authentication (MFA): Require secondary authentication for privileged accounts.

Credential Monitoring: Detect abnormal authentication patterns and block automated login attempts.

Discovered By: Team 6

Date: 23 March 2026
