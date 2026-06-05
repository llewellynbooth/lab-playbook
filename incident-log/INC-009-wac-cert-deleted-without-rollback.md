# INC-009 — WAC Certificate Deleted Without Rollback Plan

**Date:** 03/06/2026
**Severity:** P1 — Lab Down
**Status:** Resolved

## Summary
WAC became inaccessible for approximately 90 minutes after the working CA-issued
certificate was deleted during an attempt to reissue it with HTTP CDP and OCSP URLs.
The replacement certificate failed to load due to KSP provider incompatibility.

## Timeline
- WAC working on https://jumphost.lab.hybridinfra.dev — thumbprint 2AF418ED
- Decision made to reissue cert to include HTTP CDP and OCSP URL
- New cert requested using Get-Certificate — produced KSP cert (ADR-013 violation)
- Old cert deleted before new cert confirmed working — no rollback plan
- WAC failed to bind — Kestrel cannot load KSP private keys
- Root cause identified: KSP vs CSP provider — same as ADR-013
- Resolution: certreq INF with ProviderName = Microsoft RSA SChannel Cryptographic Provider
- WAC restored — final thumbprint: 7C0A95926311289FFDC6835C4F4404A64C486155

## Root Cause
1. ADR-013 and ADR-014 not consulted before touching WAC certificate
2. Working certificate deleted before replacement confirmed functional — no rollback plan

## Resolution
Reissued using certreq INF file with legacy CSP provider per ADR-014.
Granted NETWORK SERVICE read access to private key. WAC restored.

## Prevention
- Always consult ADRs before modifying WAC certificate
- Never delete a working certificate until replacement is confirmed loaded and serving
- WAC cert changes require: certreq INF, CSP provider, SAN, private key permissions
- Test replacement cert loaded BEFORE removing old cert
