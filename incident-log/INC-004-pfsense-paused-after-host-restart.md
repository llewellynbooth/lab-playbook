# INC-004 — pfSense Paused After HVHOST1 Domain Join Restart

**Date:** 20/05/2026
**Severity:** P2 — Major
**Component:** pfSense, HVHOST1
**Status:** Resolved

## Timeline
- HVHOST1 restarted after domain join
- pfSense VM entered Paused state automatically
- All inter-VLAN routing and internet access lost for entire lab
- JUMPHOST, DC1, DC2 lost internet connectivity
- Discovered via Get-VM on HVHOST1 showing State: Paused

## Root Cause
Hyper-V automatic stop action for pfSense VM set to Save/Pause rather than Shutdown.
When host restarted, VM was paused rather than shut down cleanly and auto-started.

## Resolution
Resume-VM -Name "pfSense" -ComputerName HVHOST1

## Prevention
Set pfSense VM automatic stop action to Shutdown and automatic start action to Always Start.
Always verify pfSense VM state after any host restart before testing connectivity.
Add pfSense state check to post-restart checklist.

## ADR Update Required
No — architecture unchanged. Operational procedure updated.
