**CVE-ID**: EDU-JUICESHOP-2026-T04-002
**Title**:  Sensitive Data Exposure via Insecure Directory Listing and Null Byte Injection
**Affected Lab**:  Juice Shop (`10.6.6.12`)
**Component**:   `/ftp` directory
**Severity**:  High
**CVSS Vector**:  `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N`
**CVSS Score**:  7.5
**Description**:  The application exhibits insecure directory listing at `/ftp`, as discovered via brute-force scanning (`dirb`). While the server attempts to restrict file downloads to specific extensions, this filter is bypassable using a double-encoded Null Byte (`%2500`), allowing unauthorized access to sensitive backup files.

**Proof of Concept**:

<img width="859" height="648" alt="Image" src="https://github.com/user-attachments/assets/2abbfde6-7968-4f76-838d-384b8de421c7" />

<img width="940" height="327" alt="Image" src="https://github.com/user-attachments/assets/4a464aae-461a-4350-b9d5-89be4de858ea" />

<img width="940" height="179" alt="Image" src="https://github.com/user-attachments/assets/23d39c19-e97f-476f-91de-7fee4bf9fbe7" />

Payload:

`http://10.6.6.12:3000/ftp/package.json.bak%2500.md`


**Steps to Reproduce**:
1. Run dirb against the target to locate the hidden `/ftp` directory.
2. Navigate to the directory in a browser to confirm listing is enabled.
3. Attempt to download a `.bak` file; observe the `403 Forbidden` error.
4. Append `%2500.md` to the filename in the URL to bypass the extension filter.

**Remediation**:
1. Disable directory listing in the web server configuration.
2. Move sensitive backup/configuration files to a directory outside the web root.
3. Implement strict server-side file validation that does not rely on simple extension suffix checks.


**Discovered By**:  Team 4
**Date**:  22/02/2026
