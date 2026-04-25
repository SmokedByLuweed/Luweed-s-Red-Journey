# Password reset poisoning via middleware - PortSwigger Lab 45

**Severity:** Medium
**Target:** https://0a6b00a1044bac9a81788873015b0007.web-security-academy.net

## Summary
The server uses the unvalidated X-Forwarded-Host value to construct the reset link.


## Vulnerability Details
**Type:** Host Header Poisoning
**Root Cause:** Lack of input sanitization/validation on the backend.
**Endpoint:**  https://0a6b00a1044bac9a81788873015b0007.web-security-academy.net/forgot-password?temp-forgot-password-token=jhxdynkkovw7btoa0zh5eejacvyxwhpm

## Steps to Reproduce
1. Request a password reset for user Carlos by intercepting with burp.
2. Send the request to the repeater and add a manipulated header for the exploit server's domain.
   header: ``X-Forwarded-Host: exploit-0a6b00a1044bac9a81788873015b0007.exploit-server.net``
4. After that, look in the Exploit server logs and you will find the link with the token.
5. Paste the GET request directly into the target URL. ``Endpoint``

## Proof of Concept
<br>
<img width="1920" height="1002" alt="image" src="https://github.com/user-attachments/assets/e964e970-b6b6-4f49-b2c9-a20864a87d58" />
<br>

## Result

<br>
<img width="1917" height="928" alt="image" src="https://github.com/user-attachments/assets/3b22b2b3-74b6-4bbc-937c-bc44437f5424" />

<br>



## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Sanitize the backend input to build the reset link only after confirming the rights hosts in the headers.
