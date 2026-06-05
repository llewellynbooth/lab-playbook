# ADR-016 — Playbook Deviation and Re-alignment

**Date:** 03/06/2026
**Status:** Accepted
**Updated:** 03/06/2026 — gap analysis applied, roadmap extended to Week 23

## What Happened

The original playbook build sequence (Weeks 4-8) was not followed. Sessions deviated
from the plan without explicit documentation or decision making.

| Planned Week | Planned Topic | Actual Topic | Documented? |
|---|---|---|---|
| Week 4 | GitHub repos + OIDC + Terraform | GPO baseline, domain accounts, laptop access | No |
| Week 5 | Linux Arc VM | PKI — Enterprise CA, WAC cert, auto-enrolment | No |
| Week 5b | Not planned | PKI completion — CRL, OCSP, NDES, backup | No |

## Root Cause

Two compounding failures:
1. Sessions driven by session brief rather than playbook — playbook never read at session start
2. No enforcement mechanism to flag deviations before work started

## Decision

Accept all deviations as they stand. Work done was valuable and relevant.
PKI is a core AZ-800/801 topic. Deviation was correct in substance, wrong in process.

## Gap Analysis Results (03/06/2026)

Full AZ-800 and AZ-801 objective review identified the following missing topics
not covered by the original roadmap:

| Gap | Exam Domain | Added to Roadmap |
|---|---|---|
| Entra Connect Sync, Connect Health, staged rollout | AZ-800 D1 (30-35%) | Week 7b |
| Windows Server containers, AKS on Windows Server | AZ-800 D3 (15-20%) | Week 11b |
| Azure Migrate, Storage Migration Service | AZ-801 D4 (20-25%) | Week 14b |
| DHCP, IPAM, Azure DNS, DNSSEC | AZ-800 D4 (15-20%) | Week 20 |
| Defender for Identity, BitLocker, OSConfig, Entra Password Protection | AZ-801 D1 (25-30%) | Week 19 |
| Azure Files, File Sync, Storage Spaces | AZ-800 D5 (15-20%) | Week 13 |

## Prevention

Session Start Protocol added to docs/session-start-protocol.md.
Non-negotiable: playbook read before every session. Deviations documented as ADR before work starts.

## Playbook Updates Applied

- v4.0: Domain corrected lab.local to lab.hybridinfra.dev, section 24.4 revised, Azure Local added
- v4.1: Gap analysis applied, roadmap extended to 23 weeks, exam domain coverage mapped

## Consequences

- Roadmap extended from 22 to 23 weeks
- Three new sessions added: 7b (Hybrid Identity), 11b (Containers), 14b (Migration)
- Week 19 expanded to include full security depth
- Week 20 expanded to include networking depth
- All gaps documented with target weeks
