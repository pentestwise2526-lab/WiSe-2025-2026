CVE-ID: EDU-JUICESHOP-2026-T06-012

Title: NoSQL Injection Leading to Mass Product Review Modification via Review Update API

Affected Lab: OWASP Juice Shop | Container: websploit-juice-shop | Port: 3000
Component: Product Review Update API (/rest/products/:id/reviews)
Severity: High

CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:L/I:H/A:N
CVSS Score: 7.7

Description:

The product review update functionality of the OWASP Juice Shop application is vulnerable to NoSQL Injection due to improper validation and sanitization of the id parameter supplied in the request body.

The backend logic processes user-controlled input directly within a database query used to update product reviews. Because the application fails to enforce strict data type validation, an attacker can inject MongoDB query operators into the request payload.

By replacing the expected numeric identifier with a crafted JSON object containing the $ne (not equal) operator, the attacker can manipulate the query logic to match all products rather than a single intended record.

This results in mass modification of product reviews across the database, demonstrating unauthorized data manipulation through NoSQL query injection.

Proof of Concept:

Target Endpoint:
PATCH /rest/products/:id/reviews

Example request used during testing:

http://10.6.6.12:3000/rest/products/1/reviews

Malicious Payload:
{"$ne":-1}

Result:

The application processes the manipulated request and updates reviews for multiple products simultaneously, returning a JSON response containing all modified review entries.

The challenge “NoSQL Manipulation” is marked as solved, confirming successful exploitation.

Steps to Reproduce:

Log into any valid account in OWASP Juice Shop.

Open the Web Inspector and navigate to the Network tab.

Select any product and submit a review.

Observe several API calls related to product reviews.

Identify the PUT/PATCH request responsible for submitting the review.

Navigate to the official Juice Shop source code repository and clone the repository locally.

Copy the review submission route:
http://10.6.6.12:3000/rest/products/1/reviews

Inside the cloned repository, perform a search for the route:
rep -rne "/rest/products"

Identify a PATCH request handler forwarding the request to the updateProductReviews() function.

Search for the function within the repository:
grep -rne "updateProductReviews()"

Locate the function in:
products/updateProductReviews.ts

Inspect the code and observe that the id parameter is obtained directly from the request body without proper sanitization.

Note that the request also invokes a security.isAuthorized() function before processing.

Start Burp Suite and configure the browser proxy.

Enable Intercept and edit an existing product review.

Send the intercepted PATCH request to Burp Repeater.

Modify the id parameter and replace it with the following payload:
{"$ne":-1}

Enter any text in the review message field and send the request.

Observe that the response returns a JSON body containing multiple updated product reviews, confirming the vulnerability.

Remediation:

Input Validation: Strictly validate all request parameters and enforce expected data types for identifiers.

NoSQL Query Sanitization: Reject user input containing database operators such as $ne, $gt, $or, and $where.

ORM/Query Layer Protection: Use secure query builders or ORM features that prevent direct injection of query operators.

Server-Side Authorization Enforcement: Ensure that update operations only affect the intended resource based on authenticated user permissions.

Security Testing: Perform regular security assessments to detect injection vulnerabilities within database query logic.

Discovered By: Team 6

Date: 30 March 2026
