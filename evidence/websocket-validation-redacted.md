# WebSocket Credential Validation — Redacted Evidence

## Context

This document is a sanitized validation excerpt from an authorized black-box security assessment.

The objective was to determine whether authentication material exposed through public frontend content had functional significance when presented to a WebSocket service.

Target-specific identifiers and reusable authentication material have been removed.

## Redaction Notice

The public version does not include:

- target domains or URLs
- WebSocket hostnames
- credential values
- raw authentication material
- channel identifiers
- user identifiers
- reusable connection details

The purpose of this document is to preserve the observed authentication behavior without exposing operational target information.

## Initial Validation

A WebSocket-related credential was observed in publicly delivered frontend content.

Three authentication conditions were compared.

### A. Missing Credential

Observed result:

- HTTP status: `401`
- WebSocket upgrade: not accepted

### B. Invalid Credential

Observed result:

- HTTP status: `401`
- WebSocket upgrade: not accepted

### C. Frontend-Exposed Credential

Observed result:

- HTTP status: `101 Switching Protocols`
- WebSocket upgrade: accepted

The difference between the exposed credential and both negative controls demonstrated that the exposed value had authentication significance at the time of testing.

## Capability Boundary

The successful upgrade established authenticated connection behavior only.

The assessment did not demonstrate:

- private customer data access
- authenticated publish capability
- mutation capability
- account takeover
- administrative access
- financial actions

No claim beyond the observed authentication behavior is made.

## Revalidation

A later revalidation repeated the same authentication comparison.

Observed behavior during revalidation:

- missing credential: ordinary HTTP response
- invalid credential: ordinary HTTP response
- previously exposed credential: ordinary HTTP response
- no tested condition produced HTTP `101 Switching Protocols`

The historical authentication behavior could therefore not be reproduced at the later test date.

## Interpretation

The original validation demonstrated that the frontend-exposed credential was accepted by the WebSocket authentication layer while missing and invalid credentials were rejected.

The later test did not reconfirm that behavior.

For that reason, this evidence should be interpreted as historical validation rather than proof that the same credential remains active.

## Evidence Boundary

This record supports the following conclusions:

- authentication material was publicly exposed;
- the exposed value historically produced a successful WebSocket upgrade;
- negative controls were rejected during the original validation;
- later revalidation did not reproduce the successful upgrade.

This record does not establish current private-feed access, publish capability, or broader authorization impact.

## Safety

- Authentication and handshake behavior only were tested.
- No subscription payload was sent during revalidation.
- No publish or action payload was sent.
- No customer-specific channel was intentionally targeted.
- No financial action was performed.
- No brute force was used.
- No destructive testing was performed.
- No denial-of-service activity was performed.
