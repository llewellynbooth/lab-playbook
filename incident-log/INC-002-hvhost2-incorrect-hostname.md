# INC-002 — HVHOST2 Incorrect Hostname on Initial Setup

**Date:** 18/05/2026
**Severity:** P3 — Minor
**Component:** HVHOST2
**Status:** Resolved

## Timeline
- Detected during post-install status check
- Resolved immediately with Rename-Computer command before Hyper-V install

## Root Cause
Manual data entry error during Windows Server setup.
HVHOST02 entered instead of HVHOST2.

## Resolution
Rename-Computer -NewName HVHOST2 -Force
Reboot handled by subsequent Hyper-V role install.

## Prevention
Add hostname verification as first step of post-install checklist.
Consider automating hostname setting via unattend.xml in future builds.

## ADR Update Required
No — naming convention unchanged. Human error during manual setup.
