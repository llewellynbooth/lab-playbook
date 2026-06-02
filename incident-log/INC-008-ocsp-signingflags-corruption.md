# INC-008 — OCSP SigningFlags Corrupted by Failed COM Object Attempts

**Date:** 03/06/2026
**Severity:** Medium
**Status:** Resolved

## Summary
OCSP Online Responder failed to auto-enrol for its signing certificate. SigningFlags value was corrupted to 605 by multiple failed PowerShell COM object configuration attempts, preventing the OCSP service from requesting a signing cert.

## Timeline
- OCSP role installed successfully
- Attempted configuration via PowerShell COM object (CertAdm.OCSPAdmin)
- Multiple failed attempts corrupted SigningFlags to 605 (should be 2)
- OCSP signing cert requests denied or not attempted
- MMC Online Responder snap-in showed: Bad signing certificate on Array controller
- Resolution: deleted revocation config, rebuilt via MMC wizard
- Wizard set flags correctly, signing cert auto-enrolled, status: Working

## Root Cause
Used PowerShell COM objects for initial OCSP configuration instead of the MMC wizard. COM object approach requires exact flag values and clean state. Failed attempts wrote invalid flag combinations that persisted.

## Resolution
Deleted revocation configuration entirely via MMC. Rebuilt using Add Revocation Configuration wizard which sets all flags correctly in one pass.

## Prevention
Always use the MMC Online Responder wizard for initial OCSP revocation configuration. PowerShell COM objects are valid for scripted changes to existing working configurations only. Verify SigningFlags = 2 after any programmatic change.
