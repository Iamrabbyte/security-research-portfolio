# Account Recovery Bypass — Sanitized Case Study

## Summary

During an authorized black-box security assessment, I identified an account recovery weakness affecting an authorized synthetic test account.

The public version of this case study intentionally omits the exact exploit input, endpoint paths, credentials, verification values, cookies, passwords, and raw session tokens.

## Validation Chain

### 1. Negative control

An invalid verification attempt was submitted first.

Result:

- HTTP 400
- Verification rejected

### 2. Recovery transaction

A normal account recovery transaction was initiated.

Result:

- HTTP 200

### 3. Password replacement

The server accepted a password replacement without the delivered verification code being read or used.

Result:

- HTTP 200
- Password change accepted

### 4. Authentication verification

Authentication using the tester-selected temporary password succeeded.

Result:

- HTTP 200
- Valid authenticated session returned
- Same synthetic user account confirmed

### 5. Former password check

Authentication using the previous password failed after the replacement.

Result:

- HTTP 403
- Bad login credentials

This confirmed that the account password state had actually changed.

## Restoration

After validation, the original password of the synthetic account was restored.

Normal authentication was then tested again successfully.

## Impact

If exploitable against ordinary accounts, this class of weakness could allow an attacker to replace an account password without possessing the intended verification code.

## Safety

Testing was performed only against an authorized synthetic account.

No real customer account was targeted.

No financial action was performed.

No verification email contents were accessed.

The account was restored after validation.

## Disclosure Note

Exact reproduction details and sensitive target information are intentionally excluded from this public portfolio.
