# Session Start Protocol

## Non-Negotiable. Every Session. No Exceptions.

---

## Step 1 — Read the Playbook (5 minutes)

Before any terminal is opened, before any command is run:

1. Open the current week from the revised roadmap below
2. Read the objectives out loud
3. Confirm the previous week is committed and verified in GitHub
4. If anything is outstanding from last session — document it before starting new work

The playbook is the authority. The session brief is secondary.

---

## Step 2 — Confirm Alignment

Ask and answer these three questions before proceeding:

- Does today's session match the planned week in the roadmap?
- Is there any deviation from plan? If yes — stop and document it as an ADR before work starts
- Are all open items from last session either closed or formally deferred?

If any answer is no — resolve it first. Do not proceed until aligned.

---

## Step 3 — State the Objective

Write one sentence on screen before starting:

"Today we are building X so that Y."

If you cannot complete that sentence clearly, the session is not ready to start.

---

## Scope Change Protocol

If anything comes up during the session outside the planned objectives:

1. Stop — flag it explicitly, do not silently absorb it
2. Assess — is this blocking the lesson or just interesting?
3. Decide — defer to a future week OR accept as addition to this session
4. Document — if accepted, add to workbook as unplanned work with justification
5. Continue — return to lesson objectives

Unplanned work that is not documented before it starts is treated as an incident.

---

## Learning Style — Non-Negotiable

Llewellyn is an interactive learner. This means:

- Every command gets a one-line explanation of what it does BEFORE it is run
- Every error gets a diagnosis BEFORE a fix is attempted
- Every block of work gets a "here is why we are doing this" BEFORE starting
- When something goes wrong — reason through it out loud together, do not throw commands at it
- Never copy-paste without understanding. If understanding is missing, stop and explain first
- Frustration comes from circles — same problem, different command, no explanation. Avoid this.

If the explanation does not make sense — stop and ask. That is the right call.

---

## Deviation Policy

Any deviation from the planned roadmap must be:

1. Flagged explicitly before work starts
2. Documented as an ADR with reason, trade-off, and decision
3. Added to the open items list with a target week for the deferred work
4. Committed to GitHub before the session closes

Deviations that are not documented are not decisions — they are accidents.

---

## Revised Lab Roadmap — v4.1 (Gap Analysis Applied 03/06/2026)

### Goals
- AZ-800 and AZ-801 exam domains covered with hands-on evidence
- Automate everything — Terraform, GitHub Actions, PowerShell
- Understand hybrid environments — both labs fully operational
- Become a senior engineer — tested runbooks, evidence register, architecture review

### AZ-800 Coverage Target by Domain
| Domain | Weight | Covered By |
|---|---|---|
| D1 — AD DS | 30-35% | Weeks 2, 4, 7b |
| D2 — Hybrid management | 10-15% | Weeks 3, 10, 11 |
| D3 — VMs and containers | 15-20% | Weeks 1, 11b, 15-18 |
| D4 — Networking | 15-20% | Weeks 2, 6, 7, 20 |
| D5 — Storage | 15-20% | Week 13 |

### AZ-801 Coverage Target by Domain
| Domain | Weight | Covered By |
|---|---|---|
| D1 — Security | 25-30% | Weeks 5, 5b, 19 |
| D2 — High Availability | 15-20% | Weeks 12, 15-16 |
| D3 — Disaster Recovery | 10-15% | Week 14 |
| D4 — Migration | 20-25% | Week 14b |
| D5 — Monitor and troubleshoot | 15-20% | Weeks 3, 17, 21 |

### Lab 1 — Hyper-V Lab (2x Dell OptiPlex 7090 MFF)

| Week | Focus | Status | Exam |
|---|---|---|---|
| 1 | Hyper-V setup | Complete | AZ-800 D3 |
| 2 | Active Directory | Complete | AZ-800 D1 |
| 3 | Azure Arc + AMA + monitoring | Complete | AZ-800 D2, AZ-801 D5 |
| 4 | GPO baseline, domain accounts, laptop access | Complete — deviated | AZ-800 D1 |
| 5 | PKI — Enterprise CA, WAC cert, auto-enrolment | Complete — deviated | AZ-801 D1 |
| 5b | CRL/CDP, OCSP, CA backup, NDES, web enrollment | Complete | AZ-801 D1 |
| 6 | WireGuard + DDNS + pfSense hardening + VLAN enforcement | Upcoming | AZ-800 D4 |
| 7 | Remote access validation + open items from 5b | Upcoming | AZ-800 D4 |
| 7b | Hybrid Identity — Entra Connect Sync, Connect Health, staged rollout | Upcoming | AZ-800 D1 |
| 8 | Terraform + OIDC + GitHub Actions | Upcoming | Automation |
| 9 | CI/CD pipeline + drift detection | Upcoming | Automation |
| 10 | Linux Arc VM (ARCLIN01) | Upcoming | AZ-800 D2 |
| 11 | SQL Server + Arc-enabled SQL (SQLSRV01) | Upcoming | AZ-800 D2 |
| 11b | Containers — Windows Server containers, AKS concepts | Upcoming | AZ-800 D3 |
| 12 | Failover Clustering | Upcoming | AZ-801 D2 |
| 13 | Storage — DFS, Storage Replica, Azure Files, File Sync, Storage Spaces | Upcoming | AZ-800 D5 |
| 14 | Disaster Recovery — Azure Site Recovery, Hyper-V Replica, DC1 runbook | Upcoming | AZ-801 D3 |
| 14b | Migration — Azure Migrate, Storage Migration Service | Upcoming | AZ-801 D4 |

### Lab 2 — Azure Local (2x Dell OptiPlex 7090 MFF)

| Week | Focus | Status | Exam |
|---|---|---|---|
| 15 | Azure Local foundation — network, cluster prerequisites | Upcoming | AZ-800 D3 |
| 16 | Azure Local deployment — S2D, CSVs, first VM, Arc registration | Upcoming | AZ-800 D3, AZ-801 D2 |
| 17 | Azure Local operations — monitoring, updates, live migration | Upcoming | AZ-801 D5 |
| 18 | Azure Local integration — Arc VMs, AMA, unified monitoring | Upcoming | AZ-800 D2 |

### Both Labs — Security, Operations, Capstone

| Week | Focus | Status | Exam |
|---|---|---|---|
| 19 | Security — Defender for Cloud, Defender for Identity, BitLocker, OSConfig, Entra Password Protection, JEA | Upcoming | AZ-801 D1 |
| 20 | Networking depth — DHCP, IPAM, Azure DNS, DNSSEC, site-to-site VPN | Upcoming | AZ-800 D4 |
| 21 | Monitoring and observability — dashboards, alerts, performance, event logs, failure injection | Upcoming | AZ-801 D5 |
| 22 | Full lab review — ADRs, diagrams, all runbooks tested, playbook v5.0 | Upcoming | All |
| 23 | Interview and career preparation — evidence register, mock review | Upcoming | All |

---

## Open Items

| Item | From | Target Week |
|---|---|---|
| CA offsite backup — Windows Server Backup to DC2 | Week 5b | Week 13 |
| CA backup restore test | Week 5b | Week 13 |
| NDES challenge password test | Week 5b | Week 7 |
| CRL validity period review | Week 5b | Week 7 |
| WAC-WebServer-CSP template recreation | Week 5b | Week 7 |
| Linux Arc VM — ARCLIN01 | Week 5 deviation | Week 10 |
| Terraform foundation | Week 4 deviation | Week 8 |
| OIDC auth GitHub to Azure | Week 4 deviation | Week 8 |
| Entra Connect Sync | Gap analysis 03/06/2026 | Week 7b |
| Windows Server containers | Gap analysis 03/06/2026 | Week 11b |
| Azure Migrate + Storage Migration Service | Gap analysis 03/06/2026 | Week 14b |
| DHCP + IPAM | Gap analysis 03/06/2026 | Week 20 |
| Defender for Identity | Gap analysis 03/06/2026 | Week 19 |
| BitLocker | Gap analysis 03/06/2026 | Week 19 |
| Azure DNS + DNSSEC | Gap analysis 03/06/2026 | Week 20 |
