# INC-007 — WAC v2.6 Service Failing to Start Due to Kestrel Certificate Loader

**Date:** 26/05/2026
**Severity:** P2 — Moderate
**Component:** JUMPHOST -- Windows Admin Center service
**Status:** Resolved

## Timeline
- WAC service installed successfully but crashed immediately on start
- Event log showed .NET Runtime unhandled exception in Kestrel certificate loader
- Multiple restart attempts failed -- service showed Running then immediately reverted to Stopped
- Resolved by re-enrolling certificate using certreq INF file with explicit Subject, then reinstalling WAC via GUI

## Root Cause
Two compounding issues:

1. WAC v2.6 uses ASP.NET Kestrel as its web server, not HTTP.sys. Kestrel searches for certificates by Subject name in the Local Machine store. Certificates enrolled via Get-Certificate without an explicit Subject had a blank Subject field -- Kestrel could not locate the certificate and crashed on startup.

2. Earlier certificates were enrolled using a KSP (CNG) provider template. WAC installer shows these as PreConfigurationRequired -- the service account does not have automatic access to CNG key containers, causing silent failure.

## Resolution
1. Re-enrolled certificate using certreq with an INF file specifying Subject explicitly
2. Reinstalled WAC via GUI installer -- selected new certificate thumbprint showing AutoConfigured status
3. WAC service started and remained running

## Prevention
- Always use certreq with explicit INF file for WAC certificates -- never Get-Certificate
- Verify certificate shows AutoConfigured in WAC installer before proceeding
- Check Private Key Access Control -- PreConfigurationRequired means KSP, AutoConfigured means CSP
- WAC v2 certificate binding is via appsettings.json, not netsh -- all v1 documentation is incorrect for v2

## ADR Update Required
Yes -- ADR-013 and ADR-014 document the CSP provider and certreq decisions arising from this incident.
