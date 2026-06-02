# INC-009 — WAC Certificate Deleted Without Rollback Plan

**Date:** 03/06/2026
**Severity:** High
**Status:** Resolved

## Summary
WAC became inaccessible for approximately 90 minutes after the working CA-issued certificate was deleted during an attempt to reissue it with HTTP CDP and OCSP URLs. The replacement certificate failed to load due to KSP provider incompatibility, leaving WAC unable to serve HTTPS.

## Timeline
- WAC working on https://jumphost.lab.hybridinfra.dev with thumbprint 2AF418ED
- Decision made to reissue cert to include HTTP CDP and OCSP URL
- New cert requested using Get-Certificate -- produced KSP cert (ADR-013 violation)
- Old cert deleted before new cert confirmed working
- WAC failed to bind -- Kestrel cannot load KSP private keys
- Multiple recovery attempts: appsettings.json thumbprint binding, additional cert requests
- Root cause identified: KSP vs CSP provider
- Resolution: certreq INF with ProviderName = Microsoft RSA SChannel Cryptographic Provider
- WAC restored with correct CSP cert, SAN, and OCSP URL

## Root Cause
Two compounding failures:
1. ADR-013 and ADR-014 not consulted before touching WAC certificate -- known requirement for CSP provider forgotten
2. Working certificate deleted before replacement confirmed functional -- no rollback plan

## Resolution
Reissued certificate using certreq INF file with legacy CSP provider as documented in ADR-014. Granted NETWORK SERVICE read access to private key. WAC restored.

## Prevention
- Always consult ADRs before modifying WAC certificate
- Never delete a working certificate until replacement is confirmed loaded and serving
- WAC cert changes require: certreq INF, CSP provider, SAN, private key permissions -- all four
- Test replacement cert is loaded BEFORE removing old cert
