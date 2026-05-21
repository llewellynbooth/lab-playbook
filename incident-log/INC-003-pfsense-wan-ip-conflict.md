# INC-003 — pfSense WAN IP Conflict Caused Gateway Offline

**Date:** 20/05/2026
**Severity:** P2 — Major
**Component:** pfSense WAN
**Status:** Resolved

## Timeline
- pfSense WAN set to 192.168.20.2 during initial configuration
- Gateway WANGW showing offline with 100% packet loss throughout session
- Discovered via home router ARP table — TIZEN device (Samsung TV) already holding 192.168.20.2
- Resolved by renumbering pfSense WAN to 192.168.20.20

## Root Cause
IP conflict — 192.168.20.2 already assigned to a home network device via DHCP.
Home router DHCP pool started at 192.168.20.2 covering almost the entire subnet.
No IP addressing plan existed for home vs lab subnet boundaries.

## Resolution
- Home router DHCP pool changed to start at 192.168.20.100
- pfSense WAN renumbered to 192.168.20.20
- Lab static range defined as 192.168.20.2-99

## Prevention
Define full IP addressing plan covering both home and lab subnets before any static IP assignments.
Document static assignments in playbook before building.

## ADR Update Required
Yes — ADR added to week-02 workbook. IP addressing plan added as Week 1 prerequisite.
