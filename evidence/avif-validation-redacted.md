# AVIF Security Validation — Redacted Evidence

## Context

This document is a sanitized validation excerpt from an authorized black-box security assessment.

The objective was to determine whether attacker-controlled AVIF input reached the server-side image-processing path and whether higher-impact exploitation could be safely confirmed.

Target-specific infrastructure details and reusable payload material have been intentionally removed.

## Redaction Notice

The public version does not include:

- target domains or URLs
- exact endpoint paths
- payload files
- request identifiers
- infrastructure details
- internal host information
- credentials or tokens
- reusable exploitation material

## Confirmed Processing Behavior

A benign attacker-controlled AVIF image was supplied to the application's image-processing functionality.

Observed behavior included:

- HTTP status: `200`
- server-side AVIF decoding
- conversion of AVIF input to WebP
- cache-miss behavior
- successful response from the image-processing path
- application remained responsive after validation

This confirmed that attacker-selected AVIF input reached the server-side decoder rather than being handled only by a static cache layer.

## Affected Software Context

The assessed deployment exposed a Next.js version within the affected range described by the relevant security advisory.

Relevant public references:

- Next.js advisory: `GHSA-2xp9-vwfh-vxw4`
- Underlying libheif vulnerability: `CVE-2026-84383`
- libheif advisory: `GHSA-g89c-p67h-r497`
- Fixed Next.js version: `16.3.3`

The public libheif proof of concept demonstrates a memory-corruption condition.

It is not treated here as a portable or target-independent command-execution exploit.

## Higher-Impact Validation

Additional validation did not produce evidence of:

- remote command execution
- arbitrary file read
- environment-variable disclosure
- database access
- secret extraction
- process crash
- process restart
- application instability

The application remained responsive after testing.

## Evidence Calibration

The available evidence supports the following conclusions:

- affected software was present;
- attacker-controlled AVIF input reached the image-processing path;
- server-side decoding occurred;
- the tested application remained healthy after validation.

The available evidence does not support claims of:

- confirmed remote code execution;
- confirmed arbitrary file access;
- confirmed secret extraction;
- confirmed process compromise.

For that reason, the finding is documented as vulnerable-surface validation rather than confirmed RCE.

## Revalidation

A later active validation again confirmed that attacker-selected AVIF input reached the server-side image-processing path.

The input was processed successfully and the application remained responsive.

No observable command execution, arbitrary file access, environment-variable access, crash, restart, callback, or other higher-impact behavior was produced.

## Safety Boundary

No crash-capable or destructive payload was sent to the assessed environment.

Testing stopped before actions that could reasonably introduce denial-of-service risk.

Any validation requiring direct memory corruption or code-execution proof should be reproduced only in an isolated laboratory environment with synthetic data and recovery capability.

## Conclusion

The assessment confirmed an externally reachable AVIF processing surface backed by software within a vendor-declared affected range.

Attacker-selected AVIF input was successfully processed server-side.

Higher-impact exploitation was not demonstrated and is not claimed.
