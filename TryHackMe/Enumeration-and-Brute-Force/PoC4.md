# Vulnerable Password reset login - enum.thm/predictable_tokens

**Severity:** Medium 
**Target:** http://enum.thm/labs/predictable_tokens/

## Summary

The vulnerability is the predictable token, the token is a short numeric value with low entropy, observable through the URL parameter ?token=123, making it predictable and vulnerable to brute force.
The vulnerability is identified by the token's behavior.
with that the attacker can use crunch to make a dictionary payload and then the attacker can find the new password reset

## Vulnerability Details
**Type:** Predictable Token
**Root Cause:** The token is a short numeric value with low entropy, observable through the URL parameter ?token=123, making it predictable and vulnerable to brute force.
**Endpoint:** http://enum.thm/labs/predictable_tokens/reset_password.php?token=123

## Steps to Reproduce
1. in the target click in reset password and type an email for example admin@admin.com
2. navigate to endpoint with burp intercept on and capture the request and send it to intruder.
3. after that load the otp.txt list made by crunch.
4. after find the payload with different weight, look at response and you will find your new password.
5. login with your new credentials.

## Proof of Concept
Intercept the request at the endpoint, mark the token as the payload, load otp.txt, and identify the response with a different length.


below is the crunch wordlist

```bash
crunch 3 3 -o otp.txt -t %%% -s 100 -e 200
```

## Impact
The password reset token was predictable with low entropy that enables targeted brute force, allowing an attacker to perform a brute-force using a numeric wordlist. This vulnerability could lead to unauthorized account takeover.

## Remediation
To remediate, the developer should generate cryptographically secure, high-entropy tokens, enforce expiration and single-use policies, and implement rate limiting to prevent brute-force attempts.
