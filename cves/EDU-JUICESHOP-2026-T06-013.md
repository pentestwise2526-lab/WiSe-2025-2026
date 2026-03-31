CVE-ID: EDU-JUICESHOP-2026-T06-013

Title: Poison Null Byte Injection Leading to Restricted File Download via FTP File Access Endpoint

Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: FTP File Access Endpoint (/ftp)
Severity: Medium

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:N/A:N
CVSS Score: 6.5

Description:

The FTP file access functionality in the OWASP Juice Shop application is vulnerable to a Poison Null Byte Injection attack due to improper validation of file name input during file download operations.

The application attempts to restrict file downloads by allowing only specific file extensions such as .md and .pdf. However, the backend file validation logic does not correctly handle encoded null byte characters within the request path.

An attacker can exploit this weakness by inserting a double-encoded null byte sequence (%2500) followed by a permitted file extension. When processed by the backend, the null byte prematurely terminates the string during file handling operations, causing the application to ignore the appended extension check.

As a result, the attacker can bypass the file type restriction and download sensitive backup files that were not intended to be publicly accessible, such as developer backup configuration files.

This vulnerability allows unauthorized disclosure of sensitive application files stored within the server.

Proof of Concept:

Target Endpoint:
GET /ftp

Example target URL used during testing:

http://10.6.6.12:3000/ftp/package.json.bak

Malicious Payload:

package.json.bak%2500.md

Result:

The application incorrectly validates the file extension and processes the request as an allowed .md file while internally resolving the filename as package.json.bak.

The backup file is successfully downloaded, bypassing the server-side file type restriction. The challenge “Poison Null Byte” and “Forgotten Developer Backup” are marked as solved, confirming successful exploitation.

Steps to Reproduce:

Navigate to the OWASP Juice Shop application.

In the browser address bar, access the FTP directory by visiting:
http://10.6.6.12:3000/ftp

Locate the file named package.json.bak.

Attempt to download the file directly and observe that the request results in an error due to file type restrictions.

Modify the file path in the URL by inserting a double-encoded null byte followed by an allowed extension:
package.json.bak%2500.md

Submit the modified request in the browser.

Observe that the file download succeeds despite the file type restriction.

Return to the Juice Shop homepage and observe that the challenges “Poison Null Byte” and “Forgotten Developer Backup” are marked as solved, confirming successful exploitation.

Remediation:

Input Validation: Implement strict validation and sanitization of all file path inputs to prevent injection of null byte characters.

Null Byte Handling: Reject encoded or decoded null byte sequences (%00, %2500) within file paths before processing file requests.

Secure File Extension Checks: Perform file extension validation after decoding input and ensure checks are applied to the actual file name being accessed.

Whitelist Enforcement: Implement strict whitelisting of allowed files and directories rather than relying solely on extension filtering.

Security Testing: Conduct regular security testing to detect path traversal and null byte injection vulnerabilities within file handling mechanisms.

Discovered By: Team 6

Date: 31 March 2026
