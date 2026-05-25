# Reflected DOM XSS - PortSwigger Lab 

**Severity:** Medium
**Target:** https://0a880060032fa2178002df2400100079.web-security-academy.net

## Summary
It was possible to escape the string context using a backslash-quote sequence ``\"``. The vulnerability was DOM-based XSS; the `this.responseText` was placed directly in `eval()` without sanitization.

## Vulnerability Details
**Type:** Reflected DOM XSS
**Root Cause:** the `this.responseText` was placed directly in `eval()` without sanitization.

## Steps to Reproduce

1. Navigate to the search functionality
2. in the product page, type in the URL ``\"};alert(1)//``
   

## Proof of Concept
<br>
<img width="1910" height="973" alt="Screenshot From 2026-05-21 11-35-58" src="https://github.com/user-attachments/assets/fb0e289d-423a-4190-8596-4df9eed160ed" />

<br>
<img width="1910" height="973" alt="Screenshot From 2026-05-21 12-25-41" src="https://github.com/user-attachments/assets/4b12cc0b-d197-40ba-9f71-eee403e6d676" />
<br>




## Result
<br>
<img width="1913" height="933" alt="Screenshot From 2026-05-21 12-27-14" src="https://github.com/user-attachments/assets/9c336422-f094-4301-bf35-8d65fc67af61" />
<br>

## Impact

DOM XSS allows to theft of session cookies, redirection to malicious pages, keylogging, or exfiltration of page data.

## Remediation
1. Replace eval() with JSON.parse() to safely parse the server response.
2. Never pass user-controlled data to eval().
3. Sanitize and escape backslashes on the server side before reflecting data in responses.
