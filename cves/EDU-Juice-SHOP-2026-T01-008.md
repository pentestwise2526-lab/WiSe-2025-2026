🔹 CVE ID: EDU-Juice-SHOP-2026-T01-008

🔹 Title: Insecure Direct Object Reference (IDOR) in Basket API

🔹 Affected Lab and Component

Lab Name: OWASP Juice Shop

Component: Shopping Basket API

Endpoint: /rest/basket/{id}

Parameter: id (Basket ID)

Container: bkimminich/juice-shop

Port: 3000

🔹 Vulnerability Classification

CWE: CWE‑639 – Authorization Bypass Through User‑Controlled Key

OWASP Top 10: A01:2021 – Broken Access Control

🔹 CVSS v3.1 Score

Score: 7.5 (High)

Vector:

CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N

🔹 Description

The Juice Shop application exposes a REST endpoint that retrieves shopping basket data based solely on a user‑supplied numeric 

identifier. 

The application fails to validate whether the authenticated user is authorized to access the requested basket ID.

As a result, an authenticated attacker can increment or modify the basket ID in the request path and gain unauthorized access to 

other users’ 

shopping baskets, leading to exposure of sensitive information.

🔹 Exploitation Steps

Authenticate as a normal user.

Add any product to the basket.

Intercept the following request using browser developer tools:

GET /rest/basket/1

Modify the basket ID in the request:

GET /rest/basket/2

Send the modified request.

Observe another user’s basket data in the response.

🔹 Proof of Concept (PoC)

GET /rest/basket/2 HTTP/1.1

Host: localhost:3000

Authorization: Bearer <valid_token>

🔹 Impact

Successful exploitation may allow:

Unauthorized access to other users’ shopping data

Exposure of product selections and quantities

Violation of user privacy

Potential business logic abuse

🔹 Remediation

Enforce server‑side ownership validation for all object references

Avoid exposing direct object identifiers

Implement access control checks on every request

Use indirect object references or UUIDs

Discovered By Team 1
 
Date : 1/02/2026
