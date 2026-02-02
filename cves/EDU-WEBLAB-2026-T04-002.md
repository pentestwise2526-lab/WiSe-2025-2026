**CVE-ID**: EDU-WEBLAB-2026-T04-002
**Title**:  Local File Disclosure via XML External Entity (XXE)
**Affected Lab**:  WebGoat (10.6.6.11)
**Component**:  /WebGoat/start.mvc#lesson/XXE.lesson/3
**Severity**:  High 
**CVSS Vector**:  CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N
**CVSS Score**: 7.5
**Description**:  The application’s XML parser is vulnerable to XXE injections because it is configured to resolve external entities. An attacker can define a SYSTEM entity that points to the local filesystem. When the entity is referenced in the XML body, the server replaces it with the contents of the specified path, allowing the attacker to browse the server's directory structure.

**Proof of Concept**:

<img width="750" height="404" alt="Image" src="https://github.com/user-attachments/assets/958f0b9e-da11-43e8-a830-a176fb79bc4d" />

<img width="1495" height="692" alt="Image" src="https://github.com/user-attachments/assets/33f57eba-14ef-4fce-8883-99eb6c067f52" />

Payload:
```
<?xml version="1.0"?>
<!DOCTYPE root [
<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<comment>
  <text>&xxe;</text>
</comment>
```
**Steps to Reproduce**:
1. Navigate to: http://<webgoat IP>/WebGoat/start.mvc#lesson/XXE.lesson/3. 
2. In the "Add a comment" box on that page, type a simple word like hello, and intercept the comment submission request using Burp Suite or other interceptor proxy.
3. Replace the body of the request with the payload.
4. Forward the request and observe the directory listing in the comment result.

**Remediation**:
1. Disable external Document Type Definition (DTD). [Guide](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html#general-guidance)
2. Depending on your use case, there are several other protections that can be done. Each missing part means one more hole. [Link](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html#xml-parser-security-features-matrix)
3. Prevent sending XML as the request parameter. Replace with normal HTTP POST method and use the backend to process the request.

**Discovered By**:  Team 4
**Date**:  01/02/2026
