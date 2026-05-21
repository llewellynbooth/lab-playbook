# INC-005 — vSwitch-Management Isolation Prevented Host-to-VM Communication

**Date:** 20/05/2026
**Severity:** P2 — Major
**Component:** HVHOST1, HVHOST2, Virtual Switch Architecture
**Status:** Resolved

## Timeline
- Attempted to domain join HVHOST1 and HVHOST2
- Domain join failed — DNS timeout querying lab.hybridinfra.dev
- Host management NICs on vSwitch-Management (internal switch)
- VMs on vSwitch-External (external switch)
- Devices on separate virtual switches cannot communicate regardless of VLAN tag

## Root Cause
Fundamental virtual switch architecture design error.
vSwitch-Management was an internal switch with no connection to physical network or vSwitch-External.
Host NICs on vSwitch-Management were completely isolated from VMs on vSwitch-External.

## Resolution
- Redesigned to single vSwitch-External architecture
- All VMs and host management NICs connected to vSwitch-External
- VLAN tags provide segmentation — VLAN10 management, VLAN20 WAN, VLAN50 workload
- vSwitch-Management and vSwitch-Workload internal switches deleted
- pfSense LAN adapter moved from vSwitch-Management to vSwitch-External

## Prevention
Design virtual switch architecture before any VM deployment.
Single external switch with VLAN tagging is the correct pattern for this type of lab.
Document switch architecture in Week 1 design notes.

## ADR Update Required
Yes — ADR-001 added to week-02 workbook documenting single vSwitch-External decision.
