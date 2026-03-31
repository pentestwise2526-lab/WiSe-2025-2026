**CVE-ID**: EDU-JUICESHOP-2026-T03-013

**Title**: Improper Input Validation Allows Zero-Star Feedback Submission via Client-Side Bypass

**Affected Lab**: Juice Shop

**Component**: Feedback Submission (/api/Feedbacks)

**Severity**: Medium

**CVSS Vector**: AV:N/AC:L/PR:L/UI:R/S:U/C:N/I:L/A:N

**CVSS Score**: 4.8

**Description**:
The Juice Shop application improperly validates user input for the feedback rating field by enforcing restrictions only on the client side. The UI limits users to selecting ratings between 1 and 5 stars, but this constraint is not validated on the server.
As demonstrated in the manual solution, an attacker can use browser Developer Tools to intercept and modify the outgoing network request and set the rating value to 0, which is خارج the allowed range. The backend accepts this invalid input and stores it successfully.
This vulnerability arises due to missing server-side validation and reliance on client-side controls, leading to a violation of business logic and data integrity.

**Proof of Concept**
<img width="1280" height="648" alt="image" src="https://github.com/user-attachments/assets/4424ce14-979a-401e-b7d7-a9e6e8150b78" />

<img width="2805" height="1627" alt="image" src="https://github.com/user-attachments/assets/ffb79273-af42-4428-872f-9fc3f8a31aff" />


Payload:
Modified Request (via Browser DevTools → Network tab → Edit and Resend):
{
  "comment": "Bad Review",
  "rating": 0
}

**Steps to Reproduce**:
1. Open Juice Shop in browser
2. Navigate to Customer Feedback section
3. Enter any comment and select a valid rating (e.g., 1 star)
4. Open Developer Tools (F12) → Go to Network tab
5. Submit the feedback form
6. Locate the request sent to /api/Feedbacks
7. Right-click → Edit and Resend (or Replay XHR)
8. Modify the JSON payload:  "rating": 1 → "rating": 0
9. Resend the request
10. Observe that feedback is accepted with a 0-star rating

**Remediation**:
1. Enforce strict server-side validation for all inputs
2. Reject values outside allowed range (1–5) at backend
3. Implement schema validation (e.g., Joi, JSON schema)
4. Do not rely on client-side controls for security
5. Log and monitor abnormal or malformed requests

**Discovered By**: Team 3

**Date**: 2026-03-31
