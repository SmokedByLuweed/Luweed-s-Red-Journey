# DOM XSS in document.write sink using source location.search inside a select element - PortSwigger Lab 

**Severity:** Medium
**Target:** https://0a01008604fb803480b308e1001f00bf.web-security-academy.net

## Summary
The ``storeId`` variable is stored directly in ``document.write``, and the ``storeId`` comes from ``URLSearchParams.get('storeId')`` without sanitization.


## Vulnerability Details
**Type:** DOM XSS
**Root Cause:** lack of sanitization in the location.search and the variable storeId directly into document.write

## Steps to Reproduce

1. click in a product 
2. in the product page, type in the URL ``&storeId=</select><img src=x onerror="alert(1)">``
   

## Proof of Concept
<br>
<img width="1715" height="895" alt="Screenshot From 2026-05-17 22-56-01" src="https://github.com/user-attachments/assets/15365305-715d-48e0-ab9c-fae23020984a" />

<br>
<img width="1920" height="1009" alt="Screenshot From 2026-05-17 23-13-07" src="https://github.com/user-attachments/assets/9fd76aa9-7fdb-4814-ba14-940bb6cda81b" />
<br>




## Result
<br>
<img width="1920" height="932" alt="Screenshot From 2026-05-17 23-05-24" src="https://github.com/user-attachments/assets/4594ecea-b3a7-49e8-801e-c8cc7a9d3056" />
<br>

## Impact

DOM XSS allows to theft of session cookies, redirection to malicious pages, keylogging, or exfiltration of page data.

## Remediation
Validate and sanitize the storeId value before passing it to document.write().
or replace document.write() with safe methods like textContent or createElement that do not interpret HTML.
