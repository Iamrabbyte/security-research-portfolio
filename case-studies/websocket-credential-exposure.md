# WebSocket Credential Exposure — Historical Validation

## Summary

During an authorized black-box assessment, authentication material exposed through public frontend content was observed to successfully authenticate to a WebSocket service.

The original validation showed a clear difference between the exposed credential and negative controls.

A later revalidation could not reproduce the WebSocket upgrade behavior, so the finding is presented as a historical validation rather than a currently confirmed authentication bypass.

## Initial Validation

The public frontend exposed a WebSocket-related credential.

During the original validation:

- the exposed credential resulted in a successful WebSocket upgrade;
- a request without the credential was rejected;
- a request using an invalid credential was rejected.

Observed behavior:

- exposed credential: HTTP 101 Switching Protocols
- missing credential: HTTP 401
- invalid credential: HTTP 401

This established that the exposed value had authentication significance at the time of testing.

## Revalidation

A later revalidation tested the same authentication conditions again.

At that time:

- no-token request did not establish a WebSocket session;
- invalid-token request did not establish a WebSocket session;
- previously exposed credential did not establish a WebSocket session;
- all tested cases reached the same ordinary HTTP response instead.

The original authentication behavior therefore could not be reconfirmed.

## Root Cause Analysis

The original behavior indicates that authentication material intended to control access to the WebSocket service was exposed to unauthenticated clients through publicly delivered frontend content.

Embedding reusable service credentials in client-visible resources can undermine the security boundary that the credential is intended to enforce.

However, because the original WebSocket behavior is no longer reproducible, the current validity and capabilities of the exposed credential are not assumed.

## Classification

**Primary classification: CWE-200 — Exposure of Sensitive Information to an Unauthorized Actor**

A secondary classification may apply depending on the credential's intended security role:

**CWE-522 — Insufficiently Protected Credentials**

The public evidence supports exposure and historical authentication significance.

It does not establish current private-feed access, publish capability, account access, or mutation capability.

## Impact

At the time of the original validation, possession of the frontend-exposed credential produced a successful WebSocket authentication result that was not available with missing or invalid credentials.

Potential impact would depend on what authenticated WebSocket capabilities the credential actually authorized.

The assessment did not demonstrate:

- access to a private customer account;
- access to private user data;
- authenticated mutation capability;
- publish capability;
- financial actions;
- privilege escalation.

## CVSS

No production CVSS score is assigned to this case study.

The original authentication behavior was historically validated, but the capabilities available after authentication were not demonstrated and the behavior is no longer reproducible.

Assigning confidentiality, integrity, or availability impact values would therefore require assumptions not supported by the preserved evidence.

## Remediation

Recommended actions include:

- remove reusable authentication credentials from publicly delivered frontend content;
- rotate or revoke any credential that has been publicly exposed;
- avoid embedding service secrets in client-side bundles or public runtime configuration;
- use short-lived, scoped, user-bound or session-bound authentication where appropriate;
- enforce authorization independently of possession of a frontend-visible value;
- audit build-time and runtime variables for unintended public exposure;
- revalidate WebSocket authentication after remediation.

## Validation Timeline

- **2026-09-29** — WebSocket-related credential identified in public frontend content.
- **2026-09-29** — Exposed credential produced HTTP 101 Switching Protocols.
- **2026-09-29** — Missing and invalid credential controls returned HTTP 401.
- **2026-09-30** — Revalidation performed.
- **2026-09-30** — WebSocket upgrade no longer reproduced for any tested credential condition.
- **2026-09-30** — Finding retained as historical validation only.

## Disclosure Status

The timeline above describes technical validation and revalidation activity.

No public claim is made here regarding:

- vendor acknowledgement;
- bounty-program acceptance;
- coordinated disclosure;
- remediation attribution;
- whether the later behavior change resulted from this research.

## Evidence Handling

The public version intentionally excludes:

- the credential value;
- target domains and URLs;
- connection details;
- private channel identifiers;
- raw authentication material;
- reusable operational information.

The purpose of this case study is to preserve the evidence and revalidation history without exposing a credential or target-specific attack material.

## Safety Boundary

Testing was limited to authentication and connection behavior.

No customer account was targeted.

No financial action was performed.

No publish or mutation action was performed.

No private-user channel was intentionally targeted.

No brute force, destructive testing, persistence, or denial-of-service activity was used.

## Supporting Evidence

A sanitized validation record is available here:

- [Redacted WebSocket Validation Evidence](../evidence/websocket-validation-redacted.md)

The evidence preserves the historical authentication comparison, negative controls, and later revalidation while intentionally excluding the target hostname, credential value, and reusable connection details.
