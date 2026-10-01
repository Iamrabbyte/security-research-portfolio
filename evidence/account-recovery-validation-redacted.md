# Account Recovery Validation — Redacted Evidence

## Context

This document is a sanitized validation excerpt from an authorized black-box security assessment.

The test was performed using an authorized synthetic account.

Target-specific operational details have been intentionally removed.

## Redaction Notice

The public version does not include:

- target domains or URLs
- exact endpoint paths
- credentials
- email addresses
- verification values
- passwords
- cookies
- raw session tokens
- the exact triggering input
- reusable exploitation details

The purpose of this document is to preserve the validation chain without exposing material that could be reused against the assessed system.

## Validation Chain

### A. Negative Verification Control

An invalid verification attempt was submitted first.

**Observed result:**

- HTTP status: `400`
- Server result: validation failed / invalid verification value

This established that the recovery flow normally rejected an incorrect verification attempt.

### B. Recovery Transaction Initiated

A normal account-recovery transaction was initiated for the authorized synthetic account.

**Observed result:**

- HTTP status: `200`
- Server result: recovery transaction accepted

### C. Password Replacement Accepted Without Delivered Verification Code

A password replacement request was then accepted without reading or using the delivered verification code.

**Observed result:**

- HTTP status: `200`
- Server result: password changed

The exact triggering input is intentionally redacted from this public evidence.

### D. Authentication With Tester-Selected Replacement Password

Authentication was attempted using the replacement password selected during the validation.

**Observed result:**

- HTTP status: `200`
- Valid authenticated session returned
- Same authorized synthetic account confirmed

This demonstrated that a real password-state transition had occurred.

### E. Former Password Rejected

Authentication was then attempted using the password that was valid before the replacement.

**Observed result:**

- HTTP status: `403`
- Server result: bad login credentials

This provided an additional control showing that the account password had actually changed.

## Restoration

After validation, the synthetic account was restored.

The restoration sequence included:

1. a new authorized recovery transaction;
2. restoration of the original synthetic password;
3. successful authentication using the restored password.

The final login succeeded and normal account access was re-established.

## Session Evidence

A fresh ordinary login was used only to verify the structure of the resulting session.

Observed session characteristics:

- token present: yes
- token format: JWT
- JWT structure: three dot-separated segments
- header algorithm: `HS256`
- payload claim names included:
  - `iss`
  - `sub`
  - `iat`
  - `exp`

Payload values, signature material, and the raw JWT are intentionally omitted.

## Revalidation

A later revalidation was performed against the tested tenant context.

During that revalidation:

- baseline authentication succeeded;
- the recovery transaction could still be initiated;
- invalid verification input was rejected;
- the previously observed bypass condition was also rejected;
- post-test authentication remained successful.

The previously confirmed behavior was therefore no longer reproducible in that tested context at the time of revalidation.

No claim is made regarding why the behavior changed or whether the change resulted from this research.

## Conclusion

The original validation demonstrated the following sequence:

`invalid verification rejected → recovery initiated → password replacement accepted without delivered code → replacement password login succeeded → former password rejected → account restored`

This evidence supports the account-recovery case study while intentionally excluding the exact exploit input and target-specific operational details.

## Safety

- Only an authorized synthetic account was used.
- No real customer account was targeted.
- No financial action was performed.
- No verification-email contents were accessed.
- No raw authentication token is published.
- No brute force was used.
- No destructive testing was used.
- No denial-of-service activity was performed.
- The synthetic account was restored after validation.
