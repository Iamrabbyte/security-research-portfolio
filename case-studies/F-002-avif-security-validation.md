# AVIF Security Validation — Sanitized Case Study

## Summary

During an authorized black-box security assessment, I investigated a potentially vulnerable AVIF image-processing path in a Next.js application.

The goal was to determine what could be safely confirmed without causing denial of service or overstating impact.

## Confirmed Evidence

The public frontend identified the application as using a Next.js version within the vendor-declared affected range.

A benign attacker-controlled AVIF image was sent through the application's image-processing endpoint.

Observed result:

- HTTP 200
- Server-side image decoding occurred
- AVIF input was converted to WebP
- The response was not served solely from a pre-existing cache

This confirmed that the potentially vulnerable image-processing path was externally reachable.

## Impact Validation

Additional validation did not produce evidence of:

- command execution;
- environment-variable disclosure;
- filesystem access;
- database access;
- secret extraction;
- process crash;
- application restart.

Because these higher-impact effects were not observed, the issue was not represented as confirmed remote code execution.

## Severity Calibration

A vulnerable framework version and a reachable image decoder alone are not sufficient evidence of remote code execution.

The confirmed result was limited to:

- an affected component;
- an externally reachable image-processing path;
- successful processing of attacker-selected AVIF input.

Higher-impact claims were intentionally excluded because they were not demonstrated.

## Safety Boundary

No crash-capable or destructive payload was sent.

The application remained healthy after validation.

Testing was intentionally stopped before any action that could have caused denial of service.

## Remediation

Upgrade the affected Next.js deployment to a vendor-fixed version and rebuild or redeploy the application.

If direct execution validation is required, it should be performed only against an isolated clone or laboratory environment using synthetic canary data.

## Disclosure Note

Target-identifying information, exact endpoint details, payloads, and operational infrastructure are intentionally omitted from this public portfolio.
