**CVE-ID**: EDU-SQLIBREACH-2026-T03-011  
**Title**: SQL Injection in Authentication Endpoint    
**Affected Lab**: sqli-breach (websploit-sqli-breach)    
**Component**: login page  
**Severity**: High    
**CVSS Vector**: AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N   
**CVSS Score**: 9.1  


**Description**:  
The login functionality in the sqli-breach application is vulnerable to classic SQL Injection due to unsanitized user input in the username field. User-supplied data is directly concatenated into SQL queries without parameterization, allowing attackers to manipulate query logic and bypass authentication.
- The payload admin' -- terminates the original SQL query string with a single quote and comments out the password verification clause using --
- Resulting query logic: SELECT * FROM users WHERE username='admin' --' AND password='...'  
- No input validation, escaping, or prepared statements detected

**Proof of Concept**:  

<img width="510" height="576" alt="T03-11-1" src="https://github.com/user-attachments/assets/40c4d35e-0db0-4108-9971-ad03cf71e9de" />

**Payload**:  
 ```
admin' --
```
password can be anything. 

**Steps to Reproduce**:  
1. Determine sqli-breach IP by using commands `sudo docker ps` followed by `sudo docker inspect [CONTAINERID]`. Or any other method of choice.
2. Perform nmap scan on the found IP by also enabling the default scripts: `nmap -sV -sC -T5 -p- [sqli-breachIP]`. Output will show a http service at port 5000.
3. Perform network reconnaissance using `nmap -sV -sC -T5 -p- [sqli-breachIP]` to identify open ports 5000.
4. open the link `http://[sqli-breachIP]:5000/` and log in with the payload above.
5. Observe Admin flag meaning access granted.

**Remediation**:
1. Implement input validation and sanitization on all user-supplied fields
2. Use parameterized queries (prepared statements) for all database interactions

**Discovered By**: Team 3  
**Date**: 2026-02-25
