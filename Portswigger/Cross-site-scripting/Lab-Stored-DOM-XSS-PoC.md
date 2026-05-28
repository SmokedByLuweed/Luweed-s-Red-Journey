# Stored DOM XSS - PortSwigger Lab 

**Severity:** Medium
**Target:** https://0a9a009204d9e781801c0d4b005c00ca.web-security-academy.net

## Summary
It was possible to escape the incomplete sanitization with an empty brackets ``<>`` forcing the sanitization to consume its logic on the empty pair, 
allowing the real payload to bypass it.
The vulnerability was Stored-based XSS; the `innerHTML` was placed directly with `evt.data` without sanitization.

## Vulnerability Details
**Type:** Stored DOM XSS
**Root Cause:** The server sanitization is incomplete — it only processes the first tag pair. The innerHTML sink then parses the unsanitized remainder as HTML, executing the injected payload

## Steps to Reproduce

1. Navigate to the comments functionality
2. in the blog page, type in the comments ``<><img src=x onerror ="alert(1)">``
3. go back to the blog

## Proof of Concept
<br>
<img width="1920" height="1003" alt="Screenshot From 2026-05-28 11-01-37" src="https://github.com/user-attachments/assets/1c56909a-52e7-42bf-beeb-965ab4536963" />


<br>
<img width="1920" height="1003" alt="Screenshot From 2026-05-28 11-02-14" src="https://github.com/user-attachments/assets/96b86cce-55e6-4d25-a91a-e224e1ece5bc" />
<br>




## Result
<br>
<img width="1920" height="933" alt="Screenshot From 2026-05-28 11-03-00" src="https://github.com/user-attachments/assets/693dc477-7d3d-4be9-90e5-5dcf2ff9a06d" />

<br>

## Impact

DOM XSS allows to theft of session cookies, redirection to malicious pages, keylogging, or exfiltration of page data.

## Remediation
Validate and sanitize the innerHTML value before passing it to evt.data. or replace innerHTML with safe methods like textContent or createElement that do not interpret HTML.
