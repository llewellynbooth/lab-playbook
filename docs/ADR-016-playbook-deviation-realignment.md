# ADR-016 — Playbook Deviation and Re-alignment

**Date:** 03/06/2026
**Status:** Accepted

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

1. Sessions were driven by a session brief provided at the start rather than the playbook.
   The playbook was never read at session start.
2. No enforcement mechanism existed to flag deviations before work started.

## Decision

Accept all deviations as they stand. The work done was valuable and relevant to the goals.
PKI is a core AZ-800/801 topic and the right decision for the environment. The deviation
was correct in substance but wrong in process.

## Prevention

Session Start Protocol document added to docs/session-start-protocol.md.
Non-negotiable rule: playbook is read before every session. Any deviation is documented
as an ADR before work starts.

## Playbook Updates Applied (v4.0)

- Domain corrected from lab.local to lab.hybridinfra.dev throughout
- Section 24.4 updated from Weeks 4-8 to revised Weeks 4-22 roadmap
- Azure Local Lab 2 added as Weeks 15-18
- End state summary updated to reflect both labs
- Version incremented to v4.0

## Consequences

- Linux Arc VM, Terraform, and OIDC deferred to Weeks 8-10
- All deferred items documented with target weeks in session-start-protocol.md
- No work is lost — everything done was valuable
- Process is enforced going forward via session start protocol
