# Week 4 Workbook

**Dates:** 25/05/2026
**Focus:** Network access from laptop, domain hardening, GPO security baseline, VLAN enforcement

## Problem Statement
Establish direct laptop access to JUMPHOST via RDP and WAC, create named domain user accounts, deploy a security baseline GPO covering password policy, account lockout, and audit policy, and verify VLAN enforcement between the physical network and the management VLAN.

## Constraints
- Laptop on DHCP -- 192.168.20.0/24 physical network, no static IP
- Laptop local admin password unavailable -- could not elevate PowerShell on laptop
- JUMPHOST RAM still at 4GB -- deferred from Week 3
- Work VS Azure benefit still pending

## Success Criteria
- [x] RDP from laptop to JUMPHOST (192.168.10.20:3389) working
- [x] WAC accessible from laptop browser (https://192.168.10.20)
- [x] Named domain admin account created -- lbooth.admin in Domain Admins
- [x] Standard test user created -- testuser01
- [x] WAC managing as domain account -- lbooth.admin
- [x] LAB-SecurityBaseline GPO created and linked to domain root
- [x] Password policy applied and verified -- 12 char min, 90 day max, complexity enabled
- [x] Account lockout policy applied and verified -- 5 attempts, 30 min lockout
- [x] Audit policy configured -- logon, account management, policy change, privilege use, system events
- [x] VLAN enforcement verified -- laptop cannot reach DC1 or any management VLAN host except JUMPHOST on permitted ports
- [x] AD replication healthy -- all 5 naming contexts successful

## Non-Goals
- JUMPHOST RAM upgrade -- carry forward
- PKI / internal CA -- Week 5
- WireGuard VPN -- Week 6
- Work VS Azure benefit -- ongoing

## Implementation Summary

### Network Access -- Laptop to JUMPHOST
- Diagnosed laptop could not reach JUMPHOST (192.168.10.20) -- no route for 192.168.10.0/24 on laptop
- pfSense WAN had Block private networks enabled -- incorrect for this topology as WAN faces home LAN not internet -- disabled
- Added WAN firewall rules -- Allow 192.168.20.0/24 to 192.168.10.20:3389 (RDP) and :443 (WAC)
- Could not add static route on laptop -- local admin password unavailable
- Added static route on home router (NetComm NL1901ACV) -- 192.168.10.0/24 via 192.168.20.20
- RDP and WAC from laptop confirmed working

### WAC Port Correction
- Week 2 workbook documented WAC on port 4443 -- actual installation is on port 443 (default)
- pfSense WAN rule corrected from 4443 to 443
- WAC accessible at https://192.168.10.20

### Domain User Accounts
- Created lbooth.admin -- named domain admin account, added to Domain Admins group
- Created testuser01 -- standard test user
- All subsequent WAC and admin sessions conducted as lbooth.admin

### LAB-SecurityBaseline GPO
- Created new GPO -- LAB-SecurityBaseline
- Linked to domain root -- lab.hybridinfra.dev
- Moved to Link Order 1 -- above Default Domain Policy
- Configured password policy, account lockout policy, and audit policy
- Ran gpupdate /force on DC1 via Invoke-Command -- required for domain password policy to take effect
- Verified via Get-ADDefaultDomainPasswordPolicy -- all values confirmed correct

### VLAN Enforcement Verification
- Tested from laptop -- DC1 (192.168.10.10) unreachable on any port
- Tested from laptop -- JUMPHOST:3389 and JUMPHOST:443 reachable
- pfSense WAN default deny providing enforcement -- no additional block rules required
- Default deny is stronger than explicit block -- catches all unspecified traffic

## Failures and Blast Radius

| What broke | Blast radius | Recovery |
|---|---|---|
| Block private networks on pfSense WAN -- blocked all laptop traffic | Laptop could not reach any management VLAN resource | Disabled Block private networks on WAN interface |
| No static route on laptop -- could not elevate PowerShell | Laptop routing incomplete | Added static route on home router instead |
| WAC port documented as 4443 in Week 2 -- actual port is 443 | WAC unreachable from laptop on 4443 | Corrected pfSense WAN rule to port 443 |
| gpupdate on JUMPHOST did not apply domain password policy | Password policy unchanged after initial GPO link | Ran gpupdate via Invoke-Command on DC1 |
| LAB-SecurityBaseline at Link Order 2 -- Default Domain Policy winning | Our GPO settings not applied | Moved LAB-SecurityBaseline to Link Order 1 |

## Architecture Decision Records

**ADR-008 -- Disable Block private networks on pfSense WAN**
- Decision: Block private networks disabled on pfSense WAN interface
- Context: pfSense WAN faces home LAN (192.168.20.0/24) -- not the internet. RFC 1918 blocking is appropriate when WAN faces internet, not when it faces a trusted private network
- Consequences: Home LAN traffic can reach pfSense WAN -- controlled by explicit firewall rules

**ADR-009 -- Static route on home router rather than individual devices**
- Decision: Static route for 192.168.10.0/24 via 192.168.20.20 added to NetComm home router
- Context: Could not add route on laptop (no admin rights). Router-level route propagates to all devices on 192.168.20.0/24 automatically
- Consequences: All home network devices now have a route to the management VLAN -- acceptable as pfSense firewall rules control what is actually permitted

**ADR-010 -- WAC on port 443 not 4443**
- Decision: WAC runs on default port 443
- Context: Week 2 workbook incorrectly documented port 4443 -- actual installation used default port 443
- Consequences: pfSense rule and all documentation updated to reflect port 443

**ADR-011 -- pfSense WAN default deny for VLAN enforcement**
- Decision: Rely on pfSense WAN default deny rather than adding explicit block rules
- Context: pfSense blocks all inbound WAN traffic by default unless an explicit pass rule exists. Our two allow rules are the only permitted paths -- everything else is denied implicitly
- Consequences: Stronger than explicit block -- catches all unspecified traffic automatically

## Reflections

**What I learned:**
- pfSense Block private networks must be disabled when WAN faces a private network -- designed for internet-facing WAN only
- Static routes belong on the router where possible -- fixes all devices, not just one
- Domain password policy only takes effect when GPO is refreshed on a Domain Controller -- gpupdate on a member server is not sufficient
- GPO link order matters -- lower number wins. LAB-SecurityBaseline must be Link Order 1 to override Default Domain Policy
- Default deny on pfSense WAN is a stronger control than explicit block rules
- WAC Arc status column showing Unknown does not mean Arc is broken -- Azure portal is the source of truth

**What I would do differently:**
- Document WAC port accurately at installation time -- 4443 vs 443 caused unnecessary troubleshooting
- Test laptop-to-JUMPHOST connectivity at end of Week 2 when pfSense was first configured
- Always run gpupdate on DCs after linking a domain-wide GPO, not just on member servers

## Open Items
- JUMPHOST RAM upgrade -- 4GB to 8GB
- PKI / internal CA -- Week 5
- WAC certificate -- Week 5 (currently self-signed, browser shows Not secure)
- WireGuard VPN -- Week 6
- Work VS Azure benefit -- follow up with employer
- DC2, HVHOST1, HVHOST2 showing Arc as Unknown in WAC -- confirmed Connected in Azure portal, WAC display issue only
- ARCWIN01 and ARCLIN01 VMs -- not yet deployed
- Audit logs flowing to Azure Monitor via DCR -- verify in Log Analytics next session

## Next Automation Candidate
PowerShell script to create and configure domain user accounts in bulk -- name, UPN, group membership, and OU placement in a single parameterised script. Eliminates manual ADUC steps for future user provisioning.
