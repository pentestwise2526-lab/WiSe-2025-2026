🔹 CVE ID: EDU-Maze-Walker-2026-T01-012

🔹 Title: Path Traversal Leading to Arbitrary File Read in Maze-Walker Application

🔹 Affected Lab and Component

Lab Name: maze-walker

Component: File retrieval functionality

Endpoint: /

Parameter: file

Container: websploit-maze-walker

Port: 5003
🔹 Vulnerability Classification

CWE: CWE-22 – Improper Limitation of a Pathname to a Restricted Directory (Path Traversal)

OWASP Top 10: A05:2021 – Security Misconfiguration

🔹 CVSS v3.1 Score

Score: 7.5 (High)

Vector:

CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N

🔹 Description

The Maze-Walker web application allows users to request files via the file query parameter.

The application fails to properly validate and restrict user-supplied file paths before accessing the underlying file system.

Due to the absence of path normalization and directory boundary enforcement, 

an attacker can manipulate the file parameter using directory traversal sequences (../) to escape the intended application 

directory and access sensitive system files.

This vulnerability allows unauthenticated attackers to read arbitrary files from the server’s file system, 

leading to sensitive information disclosure.

🔹 Exploitation Steps

Access the Maze-Walker application in a browser:

http://<host>:5003


Request a legitimate file to confirm normal behavior:

http://<host>:5003/?file=hello.txt


Modify the file parameter to include directory traversal sequences:

../../../../etc/passwd


Load the crafted URL in a browser:

http://<host>:5003/?file=../../../../etc/passwd


Observe the contents of the /etc/passwd file displayed in the response, confirming successful exploitation.

🔹 Proof of Concept (PoC)

GET /?file=../../../../etc/passwd

🔹 Impact

Successful exploitation may allow an attacker to:

Read sensitive operating system files

Enumerate system users

Leak application or environment information

Assist in further attacks such as privilege escalation or lateral movement

🔹 Remediation

Enforce strict allow-listing of permitted filenames

Normalize and validate file paths using canonical path resolution

Restrict file access to a fixed base directory

Run the application with least-privileged file system permissions

Discovered By: Team 1

Date: 02/02/2026
