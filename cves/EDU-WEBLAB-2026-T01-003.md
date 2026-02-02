CVE ID: EDU-WEBLAB-2026-T01-003

Title: Unauthorized Access to Administrative Panel in Hydra-Nexus

🔹 Affected Lab and Component

Lab Name: hydra-nexus

Component: Administrative dashboard

Endpoint: /admin

Container: websploit-hydra-nexus

Port: 5010

🔹 Vulnerability Classification

CWE: CWE‑284 – Improper Access Control

OWASP Top 10: A01:2021 – Broken Access Control

🔹 CVSS v3.1 Score

Score: 9.1 (Critical)

Vector:

CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H

🔹 Description

The Hydra‑Nexus application exposes the administrative panel without enforcing
proper role‑based authorization checks. Any authenticated low‑privileged user
can access administrative functionality by navigating to the admin endpoint.

The absence of function‑level authorization allows attackers to perform
privileged actions that should be restricted to administrative users only.
This represents a critical access control failure.

🔹 Exploitation Steps

Access the Hydra‑Nexus application.

Authenticate using a non‑administrative user account.

Navigate to the administrative panel endpoint.

Observe that the administrative dashboard loads successfully without access restriction.

🔹 Proof of Concept (PoC)
GET /admin


Result: Administrative interface is accessible by a low‑privileged user.

🔹 Impact

Successful exploitation may allow:

Unauthorized administrative access

Privilege escalation

User and system management manipulation

Full application compromise

🔹 Remediation

Enforce strict server‑side role‑based access control (RBAC)

Validate user roles before granting access to administrative endpoints

Apply authorization middleware to all privileged routes

Discovered By Team 1
 
Date : 31/01/2026
