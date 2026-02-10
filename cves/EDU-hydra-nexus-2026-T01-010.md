# CVE ID: EDU-hydra-nexus-2026-T01-010 | Chained OS Command Injection in Hydra-Nexus Network Tools

**Severity:** Critical (CVSS v3.1: 9.8)  
**Vector:** CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H  

**Affected Lab:** Hydra-Nexus | **Container:** websploit-hydra-nexus | **Port:** 5010  
**Component:** Network diagnostic and lookup utilities  

**Vulnerable Endpoints:**  
`/tools/ping` (host), `/tools/lookup` (domain)

**Vulnerability Classification:**  
CWE-78 – Improper Neutralization of Special Elements used in an OS Command  
OWASP Top 10: A03:2021 – Injection  

**Description:**  
The Hydra-Nexus application contains multiple OS command injection vulnerabilities within its network utility endpoints. User-supplied input is passed directly to operating system commands without sanitization or validation. An unauthenticated remote attacker can inject arbitrary commands using command separators. Injected commands are executed with root privileges, enabling full compromise of the containerized environment. Both direct and blind command injection techniques are supported, significantly increasing exploitation reliability and attack surface.

**Proof of Concept:**  
/tools/ping → Payload: `8.8.8.8; id` → Result: `uid=0(root) gid=0(root)`  
/tools/lookup → Payload: `google.com; whoami` → Result: `root`  
Blind Injection → Payload: `8.8.8.8; sleep 5` → Result: delayed response confirms execution

**Impact:**  
Arbitrary OS command execution as root; complete container compromise; unauthorized access to sensitive system and application data; persistence installation; service disruption; data exfiltration; reliable chaining with additional vulnerabilities.

**Remediation:**  
Eliminate shell-based command execution; enforce strict input allowlists; use safe system APIs; avoid invoking OS shells with user input; apply least-privilege execution; implement runtime monitoring and alerting.

**Discovered By:** Team 1 | **Date:** 01 February 2026
