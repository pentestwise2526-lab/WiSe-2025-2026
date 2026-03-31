**CVE-ID**: EDU-JUICESHOP-2026-T03-012

**Title**: Confidential Document Exposure via Unrestricted /ftp/ Directory Access

**Affected Lab**: Juice Shop

**Component**: File Storage (/ftp/ endpoint)

**Severity**: High

**CVSS Vector**: AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N

**CVSS Score**: 7.5

**Description**:
The Juice Shop application exposes confidential internal documents through an unprotected /ftp/ directory. This directory is publicly accessible without authentication, allowing attackers to directly browse and download sensitive files.
The vulnerability exists due to improper access control and insecure file storage practices. Sensitive documents like internal acquisition reports are stored in a location that is directly accessible via URL. No authentication or authorization checks are enforced before granting access.
An attacker can exploit this vulnerability by manually navigating to the /ftp/ endpoint and retrieving confidential documents, leading to significant information disclosure.

**Proof of Concept**:
<img width="2800" height="1607" alt="image" src="https://github.com/user-attachments/assets/6bcf9ffd-b90e-4dc2-89a5-d3cff956d12e" />

<img width="2815" height="1637" alt="image" src="https://github.com/user-attachments/assets/2c76794b-cf42-478c-96a3-4513aa4a10e2" />

<img width="2820" height="1155" alt="image" src="https://github.com/user-attachments/assets/55a815bf-c4d7-4df2-8879-d5b21ebec7e1" />

<img width="2800" height="1370" alt="image" src="https://github.com/user-attachments/assets/a0c7b2e2-85aa-42c6-bfca-00e0c42eb9cd" />


Payload 1:

http://[JuiceShopIP:Port]/ftp/

Effect: Accesses publicly exposed FTP directory listing

Payload 2:

http://[JuiceShopIP:Port]/ftp/acquisitions.md

Effect: Downloads confidential acquisition document


**Steps to Reproduce**:
1. Start the Juice Shop application
2. Open Side menu and click on About Us to open the page
3. Click on the link "Check out our boring terms of use if you are interested in such lame stuff" to open http://10.6.6.12:3000/ftp/legal.md
4. Then go to http://10.6.6.12:3000/ftp/
5. Observe that the directory listing is visible
6. Identify confidential files acquisitions.md
7. Click on acquisitions.md
8. Confirm that no authentication is required to access these documents

**Remediation**:
1. Restrict access to sensitive directories using proper authentication and authorization
2. Disable directory listing on the web server
3. Store confidential files outside publicly accessible directories
4. Implement strict access control checks for all file requests
5. Regularly audit exposed endpoints for sensitive data

**Discovered By**: Team 3
**Date**: 2026-04-30
