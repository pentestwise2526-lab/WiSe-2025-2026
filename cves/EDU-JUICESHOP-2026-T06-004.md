CVE-ID: EDU-JUICESHOP-2026-T06-004
Title: Information Exposure via Chatbot Persistence (Bullying/Nagging)
Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: Support Chatbot Module (Juicy)
Severity: Low
CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N
CVSS Score: 3.7

Description:
The Support Chatbot in the OWASP Juice Shop application is susceptible to an information exposure vulnerability through repetitive interaction, often referred to as "nagging" or "chatbot bullying." The bot is programmed with a logic threshold that triggers the release of sensitive information—specifically a discount coupon code—after a user repeatedly asks for the same item despite initial refusals. This demonstrates a flaw in the business logic where persistence can bypass standard promotional constraints, allowing unauthenticated users to obtain unauthorized financial benefits (discounts).

Proof of Concept:
Target URL: http://10.6.6.12:3000/#/chatbot

Payload (Message): coupon (repeated 3+ times)

Result: Initially, the bot denies the request ("I have to ask my manager," "Check our social media"). After the third consecutive request for a "coupon," the bot's logic resets and provides a valid 10% discount code: mNYT0hz3Tq.

Steps to Reproduce:

Navigate to the Chatbot section of the Juice Shop.

Type "coupon" in the chat interface and press enter.

The bot will provide a canned refusal response.

Repeat the exact same "coupon" message two more times.

On the third or fourth attempt, the bot reveals the secret coupon code as shown in the evidence.

Remediation:

Rate Limiting: Implement rate limiting on the chatbot to prevent automated or rapid-fire repetitive querying.

Logic Hardening: Remove sensitive data (like valid discount codes) from the chatbot's client-side or immediate response logic. Coupons should be distributed through verified channels (email/authenticated accounts).

State Management: Ensure the chatbot does not have a "frustration" or "persistence" counter that results in the disclosure of protected information.

Input Diversity: Require the bot to escalate to a human representative or end the session if a user repeats the same unauthorized request multiple times, rather than rewarding the behavior.

Discovered By: Team 6
Date: 14 February 2026
