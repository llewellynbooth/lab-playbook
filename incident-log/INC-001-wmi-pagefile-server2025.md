# INC-001 — WMI Pagefile Commands Failed on Server 2025

**Date:** 18/05/2026
**Severity:** P3 — Minor
**Component:** HVHOST1, HVHOST2
**Status:** Resolved

## Timeline
- Detected immediately when running pagefile reduction commands
- Resolved within 5 minutes using alternative CIM method and GUI

## Root Cause
Windows Server 2025 has deprecated some WMI methods in favour of CIM.
The Set-WmiInstance command is not compatible with Server 2025 pagefile settings.

## Resolution
Used Get-CimInstance / Set-CimInstance as alternative.
Also resolved via GUI: System Properties → Advanced → Virtual Memory.

## Prevention
Update baseline scripts to use CIM instead of WMI for pagefile management.
Document in lab-configuration/baselines/hvhost-baseline.ps1.

## ADR Update Required
No — existing decision to use PowerShell for config unchanged.
Script updated to use CIM cmdlets instead of WMI.
