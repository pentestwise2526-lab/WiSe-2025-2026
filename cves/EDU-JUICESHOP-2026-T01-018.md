EDU-JUICESHOP-2026-T01-019
SQL Injection Leading to User Credential Disclosure via Product Search API
Affected Lab and Component

Lab: OWASP Juice Shop

Component: Product Search API (/rest/product/search)

Backend Technologies:

Express.js

SQLite

Vulnerability Classification

CWE:
CWE-89 – Improper Neutralization of Special Elements used in an SQL Command (SQL Injection)

OWASP Top 10:
A03:2021 – Injection

CVSS v3.1 Score and Vector

CVSS Score: 9.8 (Critical)

Vector:
AV:N / AC:L / PR:N / UI:N / S:U / C:H / I:H / A:H

Description

A SQL Injection vulnerability exists in the product search functionality of the application. The endpoint /rest/product/search accepts user input through the q parameter and directly embeds it into an SQL query without proper sanitization or parameterized statements.

An attacker can manipulate the SQL query using crafted payloads to execute arbitrary SQL commands on the backend database.

By exploiting this vulnerability with a UNION-based SQL Injection, an attacker can retrieve sensitive information from the Users table, including user email addresses, usernames, and password hashes stored in the database.

This vulnerability allows unauthorized attackers to exfiltrate sensitive authentication data without requiring any form of authentication.

Exploitation Steps

Access the search functionality in the application.

Intercept the request using Burp Suite.

Observe the request:

GET /rest/product/search?q=<search_value>

Send the request to Burp Repeater.

Use the following SQL Injection payload to retrieve user credentials from the Users table:

non-existentvalue')) union all select 1,email,username,4,password,6,7,8,9 from Users --

The application executes the injected query and returns database records containing user credentials.

The response displays sensitive information such as:

Email addresses

Usernames

Password hashes

Proof of Concept (PoC)

Example malicious request:

GET /rest/product/search?q=non-existentvalue')) union all select 1,email,username,4,password,6,7,8,9 from Users --

Example response data returned in the application interface:

admin@juice-sh.op
jim@juice-sh.op
bender@juice-sh.op

Along with their corresponding usernames and password hashes.

Impact

Successful exploitation of this vulnerability allows an attacker to:

Extract sensitive user credentials

Enumerate registered users of the application

Obtain password hashes that could be cracked offline

Perform account takeover attacks

Gain administrative access if admin credentials are exposed

This vulnerability can lead to complete compromise of user accounts and unauthorized access to the application.

Remediation

To mitigate this vulnerability, the following security controls should be implemented:

1. Use Parameterized Queries

All SQL queries must use prepared statements to ensure user input is treated as data rather than executable code.

2. Implement Input Validation

Validate and sanitize all user inputs before processing them in database queries.

3. Disable SQL Error Disclosure

Avoid exposing SQL errors or database information in HTTP responses.

4. Use ORM Security Controls

Implement secure ORM frameworks that automatically parameterize database queries.

5. Encrypt and Hash Sensitive Data Properly

Ensure passwords are securely hashed using strong algorithms such as bcrypt with appropriate salting.

Discovered by team 1 

Date  : 05/03/2026
