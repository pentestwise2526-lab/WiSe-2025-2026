CVE-ID: EDU-JUICESHOP-2026-T06-014

Title: Hardcoded Bitcoin Wallet Address Exposure via Client-Side Redirect Logic

Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: Client-Side Payment Redirect Logic (main.js)
Severity: Medium

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:L/UI:R/S:U/C:L/I:L/A:N
CVSS Score: 5.4

Description:

The OWASP Juice Shop application exposes a hardcoded Bitcoin wallet address within client-side JavaScript used to handle redirection logic for merchandise payment methods.

During the checkout process, the application provides several payment options. Inspection of the client-side JavaScript file (main.js) reveals a redirect handler containing a query parameter (redirect?to=) that includes a static Bitcoin wallet address.

Because this information is embedded directly in the publicly accessible client-side code, any authenticated user can retrieve the wallet address by inspecting the application’s JavaScript files using browser developer tools.

An attacker could leverage this exposed information for reconnaissance purposes or potentially craft malicious redirects, phishing campaigns, or payment manipulation scenarios in which users are redirected to attacker-controlled cryptocurrency wallets.

Although this issue does not directly allow modification of application data, it represents a sensitive information disclosure vulnerability caused by improper handling of payment-related information in client-side code.

Proof of Concept:

Target Component:
Client-side JavaScript file (main.js)

Example exposed reference discovered during testing:

redirect?to=bitcoin:<wallet_address>

Observed Bitcoin wallet address:

1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm

Result:

By analyzing the client-side JavaScript, the hardcoded cryptocurrency wallet address can be identified and accessed through the redirect logic.

Opening the address in a blockchain explorer reveals the associated wallet information, including transaction history and balance. This confirms that sensitive payment-related data is exposed within publicly accessible application code.

Steps to Reproduce:

Log into the OWASP Juice Shop application.

Add several products to the shopping basket.

Navigate to the basket page and proceed to checkout.

Enter dummy information in the address form to continue the checkout process.

Select Other Methods of Payment.

Hover over the merchandise payment buttons and observe that they redirect to external store links.

Open the browser’s Web Inspector and navigate to the Debugger tab.

Locate and open the main.js file.

Search within the file for the string:
redirect?to=

Observe that a Bitcoin wallet address is embedded within the redirect URL.

Copy the wallet address and paste it into a blockchain explorer to view the wallet details and transaction history.

This confirms that sensitive payment-related information is exposed through client-side code.

Remediation:

Remove Hardcoded Secrets: Avoid embedding cryptocurrency wallet addresses or other sensitive payment information directly in client-side JavaScript.

Server-Side Handling: Implement payment handling logic exclusively on the server side to prevent exposure of sensitive data.

Environment Configuration: Store payment addresses and configuration values securely in environment variables or protected server-side configuration files.

Code Review Practices: Perform regular code reviews to ensure sensitive data is not exposed within publicly accessible resources.

Security Testing: Conduct periodic security assessments to detect sensitive information disclosure within front-end code.

Discovered By: Team 6

Date: 31 March 2026
