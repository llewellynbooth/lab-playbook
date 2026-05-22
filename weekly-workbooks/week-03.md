# Week 3 Workbook

**Dates:** 23/05/2026
**Focus:** Azure Arc onboarding, Azure Monitor, cost management

## Problem Statement
Onboard the full lab estate to Azure Arc, establish a Log Analytics workspace for centralised monitoring, and set up cost controls to protect the personal Azure subscription while work Visual Studio benefits are pending approval.

## Constraints
- Personal Azure subscription in use — work VS benefit pending, up to 1 week wait
- Work account blocked by Conditional Access policy — personal account only for Azure auth
- Australia Southeast has limited Azure service availability — impacted region selection
- JUMPHOST running at 86% memory at idle — 4GB insufficient, upgrade deferred to post-session

## Success Criteria
- [x] All 5 machines Arc-enabled and Connected in Azure portal
- [x] AMA installed and Succeeded on all machines
- [x] Data flowing into Log Analytics — confirmed via KQL heartbeat query
- [x] DCR collecting Performance Counters and Windows Event Logs
- [x] Log Analytics workspace deployed — law-lab-01
- [x] Budget alert configured — A$20 with two thresholds
- [x] All resources tagged — environment, owner, project
- [x] Arc extension auto-upgrade confirmed enabled

## Non-Goals
- Defender for Cloud — future week
- Azure Policy — future week
- KQL alerting — Week 7+
- Laptop RDP rule in pfSense — carry forward
- Domain user accounts — carry forward
- PKI and internal CA — carry forward

## Implementation Summary
- Activated personal Azure subscription — A$279 credit available
- Created resource group — rg-lab-hybridinfra-australiaeast
- Registered resource providers — HybridCompute, GuestConfiguration, HybridConnectivity, AzureArcData
- Arc-enabled all 5 machines using three different onboarding methods
- Deployed Log Analytics workspace — law-lab-01 (Australia East)
- Created Data Collection Rule — dcr-lab-windows (Australia East)
- DCR scoped to resource group level — automatically picks up future machines
- Configured budget alert — A$20/month with 50% forecast and 90% actual thresholds
- Tagged all resources — environment: lab, owner: llewellyn, project: hybrid-lab
- Verified data flowing via KQL — all 5 machines reporting heartbeat
- Confirmed AMA v1.42.0.0 installed and Succeeded on all machines
- Cleaned up expired MS Learn tenant and empty Procube Cloud Services tenant

## Onboarding Methods Used
| Machine | Method |
|---|---|
| DC1 | PowerShell script via WAC |
| DC2 | PowerShell script via WAC |
| JUMPHOST | PowerShell script via WAC |
| HVHOST1 | Direct PowerShell via RDP |
| HVHOST2 | Invoke-Command remote method |

## Arc Onboarding Methods — Full Reference
| Method | When to use |
|---|---|
| PowerShell script (manual) | One-off onboarding, full control, personal accounts |
| WAC built-in Arc settings | Quick onboarding — requires organisational account |
| Service Principal + script | Bulk onboarding at scale |
| Azure Policy | Automatic onboarding across an environment |
| Ansible / DSC | Onboarding as part of broader automation pipeline |

## Failures and Blast Radius
| What broke | Blast radius | Recovery |
|---|---|---|
| Region mismatch — Arc machines in Australia Southeast, workspace and DCR in Australia East | DCR could not find workspace in destination dropdown | Recreated workspace in Australia East — regions now consistent |
| Ctrl+C during Arc onboarding script on DC2 | Script cancelled mid-execution — agent partially installed | Re-ran script — installer handled re-run cleanly |
| Work account blocked by Conditional Access | Could not authenticate Arc agent via work account | Switched to personal Microsoft account for all Arc auth |
| WAC built-in Arc settings page blocked personal account | AADSTS500200 error — personal accounts not supported | Used PowerShell script method instead |
| DCR workspace dropdown showed no results | Region mismatch between DCR and workspace | Azure Copilot identified fix — workspace recreated in matching region |

## Reflections
**What I learned:**
- Region planning must happen before deploying anything — inconsistent regions cause cascading failures
- Australia Southeast is a secondary region with limited service availability — always use Australia East
- Multiple Arc onboarding methods exist — know them all and when to use each
- DCR scoped to resource group is more scalable than scoping to individual machines
- Separate DCRs per OS platform (Windows/Linux) is best practice — not one mixed rule
- Tags must be applied at creation time — retrofitting is inefficient
- KQL is the query language for everything in Azure Monitor — invest in learning it
- Budget alerts in Azure are notifications only — they do not hard-stop spending
- The free 5GB/month Log Analytics allowance is per billing account — lab will stay within it

**What I would do differently:**
- Plan Azure regions before creating any resources — document the decision in ADR first
- Tag every resource at creation time — not as an afterthought
- Verify WAC authentication method before starting Arc onboarding

## Architecture Decision Records (ADR)

**ADR-004 — Australia East as primary Azure region**
- Decision: All Azure resources deployed in Australia East
- Context: Australia Southeast has limited service availability — DCR workspace lookup, some portal features unavailable
- Consequences: Slight region mismatch with Arc machine registration metadata (Australia Southeast) — functionally irrelevant, data flows correctly

**ADR-005 — Separate DCR per OS platform**
- Decision: dcr-lab-windows for Windows machines, dcr-lab-linux to be created when first Linux VM deployed
- Context: Windows and Linux collect different data — separate rules are more precise and easier to manage
- Consequences: Slightly more rules to manage — offset by clarity and scalability

**ADR-006 — DCR scoped to resource group**
- Decision: DCR applied at resource group level rather than individual machines
- Context: Scoping to resource group automatically includes future machines without manual updates
- Consequences: All machines in resource group inherit the DCR — intentional and correct for lab

**ADR-007 — Personal Azure subscription for lab**
- Decision: Use personal Azure subscription while work VS benefit is pending
- Context: Work account blocked by Conditional Access — personal account required
- Consequences: Migration to work tenant when benefit activates is a re-onboarding (30 min) not a migration — no data loss, just re-registration

## Open Items
- JUMPHOST RAM upgrade — 4GB to 8GB (post-session)
- Laptop RDP access to JUMPHOST — pfSense firewall rule port 3389
- WAC accessible from laptop — pfSense rule port 4443
- Domain user accounts — named admin + standard test user
- PKI / internal CA — replace WAC self-signed certificate
- WAC managing as domain account — currently using local Administrator
- Work VS Azure benefit — follow up with employer

## Next Automation Candidate
PowerShell script to Arc-enable multiple machines in bulk using a Service Principal — eliminates interactive device login per machine, scales to any number of servers.

## First KQL Query
```kusto
Heartbeat
| take 50
```
Returns the last 50 heartbeat records from all Arc-enabled machines. All 5 machines confirmed reporting within 1 minute intervals.
