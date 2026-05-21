# DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded - PortSwigger Lab 

**Severity:** Medium
**Target:** https://0a14001d03c7fd14802203d8002100f6.web-security-academy.net

## Summary
It's possible to use ``{{ }}`` to escape the sandbox and send an alert() by manipulating the function() using constructor.constructor with an empty string.


## Vulnerability Details
**Type:** DOM XSS
**Root Cause:** Sandbox bypassable by language design

## Steps to Reproduce

1. In the search functionality put ``{{' ' .constructor.constructor('alert(1)')()}}``

   

## Proof of Concept
<br>
<img width="1918" height="1008" alt="Screenshot From 2026-05-20 13-50-18" src="https://github.com/user-attachments/assets/289a0eff-b575-404c-9c51-fe080f7474fa" />


<br>



## Result
<br>
<img width="1920" height="922" alt="Screenshot From 2026-05-20 13-50-01" src="https://github.com/user-attachments/assets/949ce721-a897-4ae9-b5fa-54785b3cd765" />
<br>

## Impact

DOM XSS allows to theft of session cookies, redirection to malicious pages, keylogging, or exfiltration of page data.

## Remediation

Switch from AngularJS to its rewritten version, Angular, released in 2016.  or 
sanitize and validate user input before rendering in the AngularJS context, and implement a Content Security Policy to restrict the execution of unauthorized scripts.
