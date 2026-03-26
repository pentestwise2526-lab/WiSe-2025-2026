CVE-ID: EDU-JUICESHOP-2026-T06-002
Title: Cross-Site Scripting (XSS) via Search Query Parameter in OWASP Juice Shop
Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: Search Results Module
Severity: High
CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N
CVSS Score: 6.1

Description:
The search functionality of the OWASP Juice Shop application is vulnerable to DOM-based Cross-Site Scripting (XSS). The application takes user-supplied input from the q URL parameter and reflects it back into the Document Object Model (DOM) without sufficient sanitization or encoding. An attacker can craft a malicious URL containing JavaScript; when a victim clicks this link, the script executes within the context of their browser session. This can be used to steal session cookies, hijack user accounts, or deface the website.

Proof of Concept:
Target URL: http://10.6.6.12:3000/#/search

Payload (URL Encoded):
?q=<iframe src="javascript:alert('xss')">

Result:
The application processes the search query and injects the <iframe> tag into the page. The src attribute then executes the JavaScript alert('xss'), resulting in a browser pop-up as shown in the provided evidence.

Steps to Reproduce:

Navigate to the OWASP Juice Shop homepage at http://10.6.6.12:3000.

In the search bar, enter the following payload: <iframe src="javascript:alert('xss')">.

Alternatively, append the payload directly to the URL: http://10.6.6.12:3000/#/search?q=<iframe src="javascript:alert('xss')">.

Observe the JavaScript alert box appearing on the screen, confirming execution.

Check the "Score Board" to verify the challenge "DOM XSS" has been marked as solved.

Remediation:

Output Encoding: Implement context-aware output encoding for all user-supplied data reflected in the DOM. Use libraries like DOMPurify to sanitize HTML before rendering.

Content Security Policy (CSP): Define and enforce a strict CSP header to prevent the execution of inline scripts and restrict the sources from which scripts can be loaded.

Use Secure Framework APIs: Instead of using dangerous methods like .innerHTML, use safer alternatives like .textContent or .innerText which do not evaluate HTML/JavaScript.

Input Validation: While not a complete fix for XSS, implement server-side and client-side validation to reject unexpected characters or tags in the search field.

Discovered By: Team 6
Date: 14 February 2026
