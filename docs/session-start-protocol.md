# Session Start Protocol

## Non-Negotiable. Every Session. No Exceptions.

---

## Step 1 — Read the Playbook (5 minutes)

Before any terminal is opened, before any command is run:

1. Open the current week from the revised roadmap in the playbook
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

## Revised Lab Roadmap

### Lab 1 — Hyper-V Lab (2x Dell OptiPlex 7090 MFF)

| Week | Focus | Status |
|---|---|---|
| 1 | Hyper-V setup | Complete |
| 2 | Active Directory | Complete |
| 3 | Azure Arc + AMA + monitoring | Complete |
| 4 | GPO baseline, domain accounts, laptop access | Complete — deviated (ADR-016) |
| 5 | PKI — Enterprise CA, WAC cert, auto-enrolment | Complete — deviated (ADR-016) |
| 5b | CRL/CDP, OCSP, CA backup, NDES, web enrollment | Complete |
| 6 | WireGuard + DDNS + pfSense hardening + VLAN enforcement | Upcoming |
| 7 | Remote access validation + open items from 5b | Upcoming |
| 8 | Terraform + OIDC + GitHub Actions | Upcoming |
| 9 | CI/CD pipeline + drift detection | Upcoming |
| 10 | Linux Arc VM (ARCLIN01) | Upcoming |
| 11 | SQL Server + Arc-enabled SQL (SQLSRV01) | Upcoming |
| 12 | Failover Clustering | Upcoming |
| 13 | Storage Replica + DFS + Windows Server Backup | Upcoming |
| 14 | Disaster Recovery + DC1 failure runbook tested | Upcoming |

### Lab 2 — Azure Local (2x Dell OptiPlex 7090 MFF)

| Week | Focus | Status |
|---|---|---|
| 15 | Azure Local foundation — network, cluster prerequisites | Upcoming |
| 16 | Azure Local deployment — S2D, CSVs, first VM, Arc registration | Upcoming |
| 17 | Azure Local operations — monitoring, updates, live migration | Upcoming |
| 18 | Azure Local integration — Arc VMs, AMA, unified monitoring | Upcoming |

### Both Labs — Security, Operations, Capstone

| Week | Focus | Status |
|---|---|---|
| 19 | Security hardening — Defender for Cloud, Key Vault, baselines | Upcoming |
| 20 | Monitoring and observability — dashboards, alerts, failure injection | Upcoming |
| 21 | Full lab review — ADRs, diagrams, runbooks tested, playbook v5.0 | Upcoming |
| 22 | Interview and career preparation — evidence register, mock review | Upcoming |

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
