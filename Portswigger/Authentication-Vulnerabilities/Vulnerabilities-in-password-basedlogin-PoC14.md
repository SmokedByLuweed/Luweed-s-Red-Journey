# Enumeration via subtly different responses - PortSwigger Lab 14

**Severity:** Medium 
**Target:** https://0a1a00e10448a2e08b96447800380068.web-security-academy.net

## Summary
This vulnerability exploits subtly different responses and notice that it contains a typo in the error message - instead of a full stop/period, there is a trailing space. 
with that an attacker can use Grep Extract in Burp and captures the exact text of the message and detects the trailing space — knowing the user, the attacker can use the password wordlist 
also the absence of rate limiting and a weak password allow an attacker to perform a brute force attack without being blocked.

## Vulnerability Details
**Type:** enumeration via subtly different responses
**Root Cause:** The error message "Invalid username or password. " contains a trailing space when the user is right but the password is wrong
**Endpoint:** https://0a1a00e10448a2e08b96447800380068.web-security-academy.net

## Steps to Reproduce - Official method

1. With Burp running, submit an invalid username and password. Highlight the username parameter in the POST /login request and send it to Burp Intruder.
2. Go to Intruder. Notice that the username parameter ``username`` is automatically marked as a payload position.
3. In the Payloads side panel, make sure that the Simple list payload type is selected and add the list of candidate usernames.
4. Click on the ``⚙`` Settings tab to open the Settings side panel. Under Grep - Extract, click Add. In the dialog that appears, scroll down through the response until you find the error message ``Invalid username or password.``.
   Use the mouse to highlight the text content of the message. The other settings will be automatically adjusted. Click OK and then start the attack.
5. When the attack is finished, notice that there is an additional column containing the error message you extracted. Sort the results using this column to notice that one of them is subtly different.
6. Look closer at this response and notice that it contains a typo in the error message - instead of a full stop/period, there is a trailing space. Make a note of this username.
7. Close the results window and go back to the Intruder tab. Insert the username you just identified and add a payload position to the ``password`` parameter: ``username=identified-user&password=§invalid-password§``
8. In the Payloads side panel, clear the list of usernames and replace it with the list of passwords. Start the attack.
9. When the attack is finished, notice that one of the requests received a ``302`` response. Make a note of this password.
10. Log in using the username and password that you identified and access the user account page to solve the lab.
    note: It's also possible to brute-force the login using a single cluster bomb attack. However, it's generally much more efficient to enumerate a valid username first if possible.

## Proof of Concept - my Approach

```bash
hydra -L /home/luweed/Documents/wordusers.txt \
-P /home/luweed/Documents/rockpass.txt \
0a1a00e10448a2e08b96447800380068.web-security-academy.net \
https-post-form \
"/login:username=^USER^&password=^PASS^:Invalid username or password."
```

user ``atlanta`` password ``2000``


## Result

<br>
<img width="1917" height="1044" alt="image" src="https://github.com/user-attachments/assets/aa179941-4d69-40e4-b152-9e1525c498cd" />




## Impact
This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
developer should put a rate limit by IP and enforce strong password policy. this should keep safe by most of the attacks.
