# Account Recovery Bypass

## Summary

During an authorized black-box assessment, I identified a weakness in an account-recovery flow using an authorized synthetic test account.

The server accepted a password replacement without the intended verification code being read or used.

The resulting password-state change was independently confirmed by authenticating with the tester-selected password and verifying that the former password was rejected.

The synthetic account was restored after validation.

## Confirmed Behavior

The validation sequence was:

1. An invalid verification value was submitted.
   - Result: HTTP 400
   - Verification rejected

2. A normal recovery transaction was initiated.
   - Result: HTTP 200

3. A password replacement was submitted without reading or using the delivered verification code.
   - Result: HTTP 200
   - Password replacement accepted

4. Authentication was attempted using the tester-selected replacement password.
   - Result: HTTP 200
   - Valid authenticated session returned

5. Authentication was attempted using the former password.
   - Result: HTTP 403
   - Former password rejected

This demonstrated an actual password-state transition rather than a client-side or cosmetic response.

## Root Cause Analysis

The observed behavior indicates that the password-recovery workflow did not strictly enforce successful verification of the recovery challenge before allowing the account password to change.

The recovery transaction and password replacement stages were therefore insufficiently bound to successful completion of the intended verification step.

## Classification

**CWE-640 — Weak Password Recovery Mechanism for Forgotten Password**

The demonstrated weakness affects the verification controls protecting the password-recovery process.

## Impact

If the same behavior were exploitable against ordinary user accounts, an unauthenticated attacker could potentially replace an account password without possessing the intended recovery verification code.

Potential consequences include:

- unauthorized password replacement;
- account takeover;
- loss of account confidentiality;
- unauthorized modification of account-controlled data.

No real customer account was used to demonstrate this impact.

## CVSS

**Illustrative CVSS v3.1 vector:**

`CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N`

**Illustrative base score: 9.1 (Critical)**

This vector represents the demonstrated security scenario assuming the behavior is reachable remotely against an ordinary account without prior authentication.

It is not presented as a vendor-assigned score.

The availability metric is left at `A:N` because broader service availability impact was not demonstrated during the assessment.

## Remediation

Recommended actions include:

- require successful server-side verification of the recovery challenge before accepting any password replacement;
- bind the recovery challenge to the specific account and recovery transaction;
- reject unexpected data types and malformed verification values;
- make recovery challenges single-use;
- enforce expiration and attempt limits;
- invalidate a recovery transaction after successful completion;
- prevent password replacement when verification state is incomplete or invalid;
- re-test the complete account-recovery flow after remediation.

## Validation Timeline

- **2026-09-29** — Initial validation performed using an authorized synthetic account.
- **2026-09-29** — Invalid verification negative control rejected.
- **2026-09-29** — Password replacement accepted without the delivered verification code being used.
- **2026-09-29** — Authentication with the replacement password succeeded.
- **2026-09-29** — Former password rejected.
- **2026-09-29** — Original synthetic account state restored.
- **2026-10-01** — Revalidation performed.
- **2026-10-01** — Previously observed bypass behavior was no longer reproducible in the tested tenant context.

## Disclosure Status

The timeline above documents technical validation and revalidation activity.

The later behavior change is not attributed to my testing or disclosure unless independent vendor acknowledgement can be provided.

No public claim is made here regarding:

- vendor acknowledgement;
- bounty-program acceptance;
- coordinated disclosure;
- remediation attribution.

## Evidence Handling

The public version intentionally excludes:

- target domains and URLs;
- exact endpoint paths;
- credentials;
- passwords;
- cookies;
- session tokens;
- recovery verification values;
- exact triggering input;
- reusable exploitation material.

The public case study preserves the validation logic and observed state transitions without exposing target-specific operational details.

## Safety Boundary

Testing was limited to an authorized synthetic account.

No real customer account was targeted.

No verification-email contents were accessed.

No financial action was performed.

No persistence, brute force, destructive testing, or denial-of-service activity was used.

The synthetic account was restored after validation.


## Supporting Evidence

A sanitized validation record is available here:

- [Redacted Account Recovery Validation Evidence](../evidence/account-recovery-validation-redacted.md)

The evidence preserves the observed HTTP status transitions, password-state change, restoration sequence, and later revalidation while intentionally excluding target-specific operational details and the exact triggering input.
