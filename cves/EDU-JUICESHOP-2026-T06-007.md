CVE-ID: EDU-JUICESHOP-2026-T06-007

Title: Account Takeover via OSINT-Based Security Question Weakness in Forgot Password Mechanism

Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: Password Reset Functionality
Severity: High

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N
CVSS Score: 8.8

Description:

The password reset mechanism of the OWASP Juice Shop application is vulnerable to account takeover due to reliance on weak knowledge-based authentication using publicly discoverable security questions.

The application allows password resets based solely on answering a predefined security question without implementing additional identity verification controls. The answer to the security question (“Name of your favorite pet”) can be discovered through Open-Source Intelligence (OSINT) gathered from publicly accessible social media content linked within the application.

An unauthenticated attacker can enumerate user information, identify publicly available personal details, and successfully reset the victim’s password. This results in full account compromise without requiring prior authentication or technical exploitation.

The vulnerability demonstrates insecure authentication design where sensitive account recovery relies on easily obtainable personal information.

Proof of Concept:

Attack Method:
The attacker gathers publicly available information associated with the target user through links embedded within the application.

Result:
After correctly answering the security question using OSINT-derived information, the attacker successfully resets the account password. The challenge “Bjoern's Favorite Pet” is marked as solved, confirming successful account takeover.

Steps to Reproduce:

Navigate to the Forgot Password page in OWASP Juice Shop.

Enter the email address: bjoern@owasp.org

Identify the security question asking for the name of Bjoern’s favorite pet.

Perform OSINT reconnaissance: Go to the Photo Wall section of the application.

Locate a post created by bkimminich.

Open the Twitter link referenced in the post, which leads to Bjoern’s public Twitter account.

Scroll through the profile to find a post referencing a pet cat named: Zaya

Enter Zaya as the answer to the security question and choose any new password.

Submit the request and observe successful password reset and challenge completion.

Remediation:

Strong Authentication for Password Reset: Avoid using knowledge-based authentication questions as a sole verification method.

Multi-Factor Verification: Require email verification links or multi-factor authentication (MFA) during password recovery.

Reduce OSINT Exposure: Avoid referencing real-world social media accounts or publicly accessible personal data for authentication.

Rate Limiting & Monitoring: Implement attempt limits and anomaly detection on password reset functionality.

Secure Account Recovery Design: Use time-limited reset tokens generated server-side instead of static answers.

Discovered By: Team 6

Date: 2 March 2026
