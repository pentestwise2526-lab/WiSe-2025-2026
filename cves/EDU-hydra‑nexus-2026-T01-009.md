# CVE Report

**CVE ID:** EDU-hydra‑nexus-2026-T01-009
**Title:** Improper File Upload Validation and Public Access to Uploaded Files in Hydra‑Nexus
**Severity:** High
**CVSS v3.1 Score:** 7.5
**CVSS Vector:** CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N

---

## Affected Product and Component

* **Lab Name:** hydra‑nexus
* **Component:** File upload functionality
* **Endpoint:** `/upload`
* **Upload Directory:** `/uploads/`
* **Container/Image:** websploit‑hydra‑nexus
* **Service Port:** 5010

---

## Vulnerability Classification

* **CWE‑434:** Unrestricted Upload of File with Dangerous Type
* **CWE‑200:** Exposure of Sensitive Information to an Unauthorized Actor

**OWASP Top 10 (2021):**

* A05 – Security Misconfiguration
* A01 – Broken Access Control

---

## Description

The Hydra‑Nexus application contains multiple security weaknesses in its file upload implementation at the `/upload` endpoint. The application fails to enforce strict server‑side validation of uploaded files, allowing files with potentially dangerous extensions to be uploaded using misleading filenames (e.g., double extensions).

In addition, after a successful upload, the application discloses the internal server‑side upload directory path in the response. Uploaded files are stored within a publicly accessible directory and can be retrieved directly via URL without any authentication or authorization checks.

Although server‑side execution of PHP files is disabled within the upload directory, this does not sufficiently mitigate the risk. Attackers can still leverage this weakness to host malicious content, distribute malware, access sensitive uploaded data, and gather internal directory structure information for further attacks.

---

## Attack Vector and Exploitation Scenario

* **Attack Vector:** Network‑based (Remote)
* **Authentication Required:** None
* **User Interaction:** Not required

An attacker can upload a malicious or sensitive file and then directly access or distribute it via the publicly exposed uploads directory. The lack of access control and path disclosure significantly lowers the barrier for abuse and vulnerability chaining.

---

## Proof of Concept (PoC)

**Uploaded File Content:**

```
<?php system($_GET['cmd']); ?>
```

**Upload Filename:**

```
shell.php.jpg
```

**Access Request:**

```
GET /uploads/shell.php.jpg
```

**Observed Result:**
The uploaded file is successfully downloaded without authentication, confirming unrestricted file upload acceptance and improper access control on uploaded content.

---

## Impact

Successful exploitation of this vulnerability may result in:

* Upload and distribution of malicious files
* Unauthorized access to sensitive uploaded data
* Disclosure of internal application directory structure
* Bypass of access control mechanisms
* Chaining with other vulnerabilities (e.g., insecure backup restore, deserialization issues) leading to full application compromise

While direct code execution is not immediately possible, the overall security impact remains high due to data exposure and attack surface expansion.

---

## Remediation and Mitigation

To mitigate this vulnerability, the following security controls are recommended:

* Enforce strict server‑side file validation and allowlisting of safe file types
* Block dangerous file extensions regardless of MIME type or filename
* Store uploaded files outside the web root
* Disable direct public access to uploaded files
* Avoid disclosing internal directory paths in application responses
* Implement authentication and authorization checks for all file access operations

---

## Discovery Details

* **Discovered By:** Team 1
* **Discovery Date:** 01/02/2026


