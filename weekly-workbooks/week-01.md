# Week 1 Workbook

**Dates:** 18/05/2026
**Focus:** Physical setup and Hyper-V

## Problem Statement
Get both OptiPlex 7070 Tower nodes running as Hyper-V hosts
with correct network configuration and virtual switch design.

## Constraints
- No managed switch yet — using home router temporarily
- Single NVMe drive per host — OS and VMs sharing one disk
- Wireless keyboard issue on initial boot — resolved with wired keyboard

## Success Criteria
- [x] Both nodes have internet access
- [x] Hyper-V Manager opens with no errors on both nodes
- [x] All four virtual switches created on both hosts
- [x] D:\ formatted and VM folders created
- [x] Both nodes reachable via RDP from laptop

## Implementation Summary
- Downloaded Windows Server 2025 Datacenter Evaluation ISO
- Created bootable USB via Rufus on Windows 11 machine
- Installed Windows Server 2025 Datacenter Desktop Experience on both nodes
- Set hostnames HVHOST1 / HVHOST2
- Set static IPs 192.168.20.10 / 192.168.20.11
- Enabled RDP on both hosts
- Reduced page file to 8GB on both hosts
- Ran Windows Update — fully patched
- Installed Hyper-V role with management tools on both hosts
- Created D:\ partition — 889GB NTFS labelled VMs
- Created VM folder structure on both hosts
- Created four virtual switches on each host:
  - vSwitch-External (bound to Intel I219-LM)
  - vSwitch-Management (Internal)
  - vSwitch-Workload (Internal)
  - vSwitch-Private (Private)

## Failures and Blast Radius
- WMI pagefile commands failed on Server 2025 — used CIM/GUI instead
- HVHOST2 initially named HVHOST02 — corrected before Hyper-V install
- NIC driver not in base install — resolved automatically after Windows Update
- Initial wireless keyboard not detected during boot — resolved with wired USB keyboard

## Blast Radius
All failures were contained to individual host configuration.
No data loss. No impact to other systems.

## Reflections
- Having the detailed build sequence in the playbook made execution
  fast and confident — completed Week 1 in a single session
- Both hosts configured identically — good discipline established early
- Intel I219-LM NIC confirmed on both hosts — excellent Hyper-V compatibility
- ORICO NVMe drives installed — performing well

## Next Automation Candidate
PowerShell baseline script to automate full Hyper-V host configuration:
- Hostname, static IP, RDP, page file, Hyper-V role, D:\ partition,
  VM folders, and virtual switches — all in one repeatable script
- Store in lab-configuration/baselines/hvhost-baseline.ps1
