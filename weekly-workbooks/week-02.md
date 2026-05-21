Week 2 — Active Directory, Network Architecture, Jumphost

Dates: 20 May 2026

Focus: Active Directory, VLAN routing, Jumphost deployment, network architecture



Problem Statement

Complete the AD foundation with DC2 replication verified, deploy a Jumphost for centralised management, configure pfSense for VLAN routing, and establish a clean network architecture for the lab.



Constraints



Cost: No additional hardware — work within existing two-host setup

Risk: Network changes affect all running VMs — pfSense downtime impacts entire lab

Tooling: Hyper-V, pfSense, Windows Server 2025 Eval, WAC





Success Criteria



DC1 and DC2 replicating with 0 failures

Jumphost domain joined with RSAT, GPMC, Hyper-V tools, and WAC installed

All servers routing internet through pfSense

HVHOST1 and HVHOST2 domain joined and manageable via WAC

All clocks synced to correct Melbourne time

pfSense WAN static IP configured and gateway online





Non-Goals



Azure Arc onboarding — Week 3

PKI and internal CA — future week

WireGuard VPN — future week

Automation pipelines — future week





Design Notes

Original plan had separate internal switches (vSwitch-Management, vSwitch-Workload). Discovered during implementation that VMs and host NICs on separate switches cannot communicate. Redesigned to single vSwitch-External carrying all VLANs via 802.1Q tagging. pfSense handles inter-VLAN routing. Host management NICs added to vSwitch-External tagged VLAN10. East-west traffic within VLAN10 bypasses pfSense — Windows Firewall is the control plane within the subnet.



Implementation Summary



AD replication verified — all 5 naming contexts replicating successfully

DC2 DNS set — preferred self, alternate DC1

Windows Firewall rules enabled on DC1 — all AD DS inbound rule groups

DNS forwarders set on DC1 and DC2 — 8.8.8.8 and 1.1.1.1

pfSense WAN set to 192.168.20.20, gateway 192.168.20.1

pfSense admin password changed

Jumphost deployed on HVHOST2 — 192.168.10.20, VLAN10, domain joined

RSAT, GPMC, Hyper-V tools, WAC installed on Jumphost — port 4443

All VMs moved to vSwitch-External with VLAN tags

HVHOST1 and HVHOST2 domain joined — 192.168.10.50 and 192.168.10.51

NTP configured on DC1 — pool.ntp.org, all domain members sync automatically

Home network redesigned — DHCP pool 192.168.20.100-254, lab statics 192.168.20.2-99

Router hardened — TR-069 disabled, HTTP/ICMP WAN disabled, ALG cleaned up

Router firmware updated — Spintel R6B016 to Netcomm R6B033

Switch firmware verified — already on latest





Failure and Blast Radius

What brokeBlast radiusRecoveryIP conflict on 192.168.20.2pfSense WAN gateway offline — no internet routing through pfSenseRenumbered pfSense WAN to 192.168.20.20pfSense paused after HVHOST1 domain join restartComplete loss of inter-VLAN routing and internet for all lab VMsManually resumed via Hyper-V consolevSwitch-Management isolationHost NICs could not communicate with VMsRedesigned to single vSwitch-External architecture



Operational Readiness



Health measured by: repadmin /showrepl, pfSense Status → Gateways, WAC dashboard

Failures detected by: pfSense gateway going offline, AD replication errors, time skew

3AM scenario: pfSense paused or stopped — resume via Hyper-V console on HVHOST1. Physical Ethernet on hosts provides out-of-band access if VLAN10 is unreachable





Reflections

What I learned:



Virtual switch architecture must be designed before deployment — single vSwitch-External is simpler and more correct than separate internal switches

IP addressing across home and lab networks must be planned together to avoid conflicts

netsh is more reliable than PowerShell New-NetIPAddress for static IP assignment on these hosts

East-west traffic within a VLAN bypasses the perimeter firewall — host-based firewall rules are essential

pfSense is a VM dependency — always verify its state after host changes



What I would do differently:



Resolve virtual switch architecture and IP addressing plan in Week 1 before any VMs are deployed

Test pfSense gateway connectivity before moving on to other tasks

Document the single vSwitch-External design decision as an ADR before building





Architecture Decision Records (ADR)

ADR-001 — Single vSwitch-External architecture



Decision: All VMs and host NICs connect to vSwitch-External only

Context: Original design used separate internal switches causing VM-to-host isolation

Consequences: Simpler architecture, all traffic on one switch, VLAN tags provide segmentation



ADR-002 — Jumphost placed on HVHOST2



Decision: Jumphost deployed on HVHOST2 instead of HVHOST1 as originally planned

Context: HVHOST1 already running DC1 and pfSense — HVHOST2 had more available RAM

Consequences: Better load distribution across hosts



ADR-003 — HVHOST1 and HVHOST2 domain joined



Decision: Both Hyper-V hosts joined lab.hybridinfra.dev

Context: Required for WAC management and centralised authentication

Consequences: Hosts behind pfSense — physical Ethernet retained as out-of-band break-glass path





Risk Register

DecisionRiskImpactMitigationUPnP enabled on home routerDevice opens unexpected portLow — pfSense isolates labAccepted — family usabilityHTTP WAN disabled on routerRemote admin not possibleLow — VPN will replace thisAcceptableICMP WAN disabled on routerRouter not pingable from internetLowAcceptableSIP/H323/IRC/PPTP ALG disabledLegacy protocols may breakNone expectedAcceptableEast-west traffic bypasses pfSenseIntra-VLAN traffic uncontrolled by firewallMediumWindows Firewall on each VMUpdate management — independentInconsistent patch stateLow — temporaryMigrate to Azure Update Manager with Arc



Open Items



Laptop RDP access to JUMPHOST — DHCP reservation on home router + pfSense firewall rule port 3389

WAC accessible from laptop browser — pfSense rule port 4443

Domain user accounts — named admin + standard test user

Update management — independent Windows Update until Azure Arc

PKI / internal CA — replace WAC self-signed certificate

Home router SSID rename — BoothHQ

IoT and guest VLAN on home router





Next Automation Candidate

Domain join script for new VMs — automate hostname, static IP, DNS, and domain join in a single PowerShell script to avoid manual steps and reduce errors on future VM deployments.



Future Week Added

Week 4 — Networking fundamentals and pfSense deep dive



VLAN trunking and 802.1Q in depth

pfSense firewall rules — inbound, outbound, floating

NAT types — static, port forward, outbound

Static routing and pfSense routing table

DHCP design — scopes, reservations, relay agents

DNS in depth — conditional forwarders, split-brain

NTP hierarchy and Stratum levels

East-west traffic and micro-segmentation concepts

Network troubleshooting methodology — OSI layer by layer

STP basics on the managed switch

