# INC-006 — Inbox WAC Feature Blocking Standalone WAC Installer

**Date:** 26/05/2026
**Severity:** P3 — Minor
**Component:** JUMPHOST -- Windows Admin Center
**Status:** Resolved

## Timeline
- Discovered when standalone WAC MSI returned error 1620 (invalid package) despite correct file size
- Resolved by removing the inbox WAC Windows feature before running standalone installer

## Root Cause
Windows Server 2025 includes WAC as an inbox optional feature (WindowsAdminCenterSetup).
The inbox feature registers itself as the WAC component and blocks the standalone MSI installer from running.
Error 1620 was misleading -- the MSI file was valid but the installer was blocked by the existing registration.

## Resolution
Removed the inbox WAC feature before running the standalone installer:

```powershell
Remove-WindowsFeature -Name WindowsAdminCenterSetup
```

Then ran the standalone MSI installer successfully.

## Prevention
When installing standalone WAC on Windows Server 2025, always remove the inbox feature first.
Document in lab build runbook -- WAC standalone install requires inbox feature removal on Server 2025.

## ADR Update Required
No -- ADR-012 onwards covers WAC certificate decisions. No architecture change required.
