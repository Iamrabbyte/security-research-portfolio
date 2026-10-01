# Account Recovery Bypass — Sanitized Case Study

## Summary

During an authorized black-box security assessment, I identified an account recovery weakness affecting an authorized synthetic test account.

Exact reproduction details and sensitive target information are intentionally omitted from this public version.

## Validation Chain

### 1. Negative Control

An invalid verification attempt was submitted.

Result:

- HTTP 400
- Verification rejected

### 2. Recovery Transaction

A normal account recovery transaction was initiated.

Result:

- HTTP 200

### 3. Password Replacement

The server accepted a password replacement without the delivered verification code being read or used.

Result:

- HTTP 200
- Password change accepted

### 4. Authentication Verification

Authentication using the tester-selected temporary password succeeded.

Result:

- HTTP 200
- Valid authenticated session returned
- Same synthetic user account confirmed

### 5. Former Password Check

Authentication using the former password failed after the replacement.

Result:

- HTTP 403
- Bad login credentials

This confirmed that the password state had actually changed.

## Restoration

After validation, the original password of the synthetic test account was restored.

Normal authentication was then tested successfully.

## Impact

If exploitable against ordinary accounts, this class of weakness could allow an attacker to replace an account password without possessing the intended verification code.

## Safety

Testing was performed only against an authorized synthetic account.

No real customer account was targeted.

No financial action was performed.

No verification email contents were accessed.

The synthetic account was restored after validation.

## Disclosure Note

The exploit input, endpoint paths, credentials, verification values, passwords, cookies, and raw session tokens are intentionally excluded from this public portfolio.
