**CVE-ID**: EDU-JUICESHOP-2026-T03-014

**Title**: Information Disclosure via Exposed Security Policy File (security.txt)

**Affected Lab**: Juice Shop

**Component**: .well-known/security.txt endpoint

**Severity**: Low

**CVSS Vector**: AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N

**CVSS Score**: 3.7

**Description**:
The Juice Shop application exposes a security policy file (security.txt) located in the standardized .well-known directory. This file is publicly accessible without authentication and contains information about vulnerability disclosure processes, contact details, and potentially sensitive metadata.
According to best practices, ethical hackers are expected to check for a security policy before performing any testing. However, improper handling or excessive disclosure within this file can provide attackers with useful reconnaissance information.
The vulnerability arises due to:
Public exposure of internal security contact and policy information
Lack of access restriction on .well-known/security.txt
Potential leakage of operational or security-related metadata
An attacker can directly access this file and gather useful intelligence for further attacks or social engineering.

**Proof of Concept**:

<img width="2810" height="1630" alt="image" src="https://github.com/user-attachments/assets/a609ace8-7af4-4d74-be09-e649cfa6f02d" />

<img width="2800" height="1640" alt="image" src="https://github.com/user-attachments/assets/382ad227-041a-47d4-bd72-49f4ef79bc83" />

Payload:

http://10.6.6.12:3000/.well-known/security.txt

Effect: Accesses exposed security policy file

**Steps to Reproduce**:
1. Start the Juice Shop application
2. Open the web application in a browser
3. Navigate to the following endpoint: http://10.6.6.12:3000/.well-known/security.txt
4. Observe that the file is publicly accessible
5. Review the contents of the file (contact info, disclosure policy)
6. Confirm that no authentication is required

**Remediation**:
1. Limit exposed information in security.txt to only necessary disclosure details
2. Avoid including sensitive internal data or personal contact information
3. Implement monitoring for abuse of disclosed contact channels
4. Use generic security contact emails instead of personal ones
5. Regularly review .well-known directory for unintended exposure

**Discovered By**: Team 3

**Date**: 2026-03-31
