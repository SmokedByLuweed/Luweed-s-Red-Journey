# SQL injection with filter bypass via XML encoding - PortSwigger Lab 

**Severity:** Medium
**Target:** https://0a8700010463534380bdb75e009c001c.web-security-academy.net

## Summary
The application WAF didn't notice encoded SQLi payloads


## Vulnerability Details
**Type:** bypass WAF via XML encoding
**Root Cause:** The WAF filters plaintext SQL keywords but fails to decode HTML entities before inspection, allowing encoded payloads to bypass detection.

## Steps to Reproduce
``Only Burp``
1. Capture the request of stockCheck in POST /product/stock
2. Encode in HTML with decoder the payload ``1 UNION SELECT NULL--`` to confirm the query returns a single column.
3. After identifying it as one, encode the final SQLi with ``1 UNION SELECT CONCAT(username,'~',password) FROM users--``
4. Login in to the administrator account


## Proof of Concept
<img width="1835" height="949" alt="Screenshot From 2026-05-03 21-06-09" src="https://github.com/user-attachments/assets/c09fe7de-8e4f-4647-a495-9c1040fda878" />
<br>
<img width="1854" height="927" alt="Screenshot From 2026-05-03 12-15-01" src="https://github.com/user-attachments/assets/6c16e66e-a851-429f-8c4f-435759132290" />

<br>
<br>
<img width="1859" height="954" alt="Screenshot From 2026-05-03 12-23-51" src="https://github.com/user-attachments/assets/06874fd6-0967-4f59-9c23-cb6a55480d34" />




## Result

<br>
<img width="1859" height="954" alt="Screenshot From 2026-05-03 12-24-01" src="https://github.com/user-attachments/assets/40ab0978-b769-434b-b33f-a5b0bb4fd652" />


<br>
<br>
<img width="1920" height="931" alt="image" src="https://github.com/user-attachments/assets/7e48907e-7502-44f9-944d-0c2ece838dde" />


<br>
<br>


user: administrator password: a28t0caqw6sx38yibyw3


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Use parameterized queries (prepared statements) to prevent user input from being interpreted as SQL. 
