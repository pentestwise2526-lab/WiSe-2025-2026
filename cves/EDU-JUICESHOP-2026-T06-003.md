CVE-ID: EDU-JUICESHOP-2026-T06-003
Title: Bonus Payload Execution via DOM XSS in Search Module Affected
Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: Search Results Module
Severity: Medium
CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:N/I:L/A:N
CVSS Score: 4.3

Description:
The application's search results page is vulnerable to an advanced DOM-based Cross-Site Scripting (XSS) attack through the q parameter. While the primary risk of XSS is malicious script execution, this specific "Bonus Payload" demonstrates the ability to inject complex, third-party HTML elements (specifically an <iframe>) directly into the application's interface. By supplying a crafted SoundCloud embed code, an attacker can force the application to load and play unauthorized external media automatically. This confirms that the application does not properly sanitize or restrict HTML tags in the search query before rendering them in the DOM.

Proof of Concept:
Target URL: http://10.6.6.12:3000/#/search

Payload:

<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>
Result: The application interprets the payload as valid HTML and embeds the SoundCloud player directly into the "Search Results" section. The auto_play=true parameter in the source URL causes the audio to trigger immediately upon page load.

Steps to Reproduce:

Navigate to the OWASP Juice Shop search interface.

Paste the SoundCloud <iframe> payload into the search bar.

Execute the search.

Observe the "Search Results" header updated with a functional SoundCloud music player.

Verify the notification: "You successfully solved a challenge: Bonus Payload".

Remediation:

Strict Content Security Policy (CSP): Implement a CSP that restricts the frame-src directive to trusted domains only. This would prevent the browser from loading iframes from unauthorized sources like soundcloud.com.

HTML Sanitization: Utilize a library like DOMPurify to strip out potentially dangerous tags (like <iframe>, <script>, and ) from user-supplied strings before they are rendered in the DOM.

Context-Aware Encoding: Ensure that parameters reflected in the UI are treated as plain text rather than executable HTML. In Angular (which Juice Shop uses), ensure that [innerHTML] is not used with untrusted data; use standard data binding {{ }} instead.

Discovered By: Team 6
Date: 14 February 2026
