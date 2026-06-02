# ADR-015 — Single-Tier PKI Accepted for Lab, Two-Tier Documented for Production

**Date:** 03/06/2026
**Status:** Accepted

## Decision
Deploy single-tier PKI with CA co-located on DC1. No offline Root CA.

## Reason
RAM and infrastructure constraints make a dedicated offline Root CA VM impractical in the lab environment. Risk is accepted for a non-production environment.

## Two-Tier Architecture (Production Standard)

### Design
- **Tier 1 — Offline Root CA:** Air-gapped machine, powers on only to sign subordinate CA cert or CRL. Private key never touches a network. Ideally stored on HSM.
- **Tier 2 — Subordinate (Issuing) CA:** Online, issues all end-entity certificates day to day.

### Why It Matters
The Root CA private key is the ultimate trust anchor for the entire PKI. If the issuing CA is compromised, it can be revoked by the Root CA and replaced. The Root CA key remains safe. In a single-tier design, a compromised CA means the entire PKI must be rebuilt from scratch.

### When Two-Tier Is Required
- Any production environment handling sensitive systems
- Public-facing PKI
- Regulated industries — healthcare, finance, government
- Anywhere the cost of a CA compromise is high

### When Single-Tier Is Acceptable
- Home labs
- Small isolated internal networks
- Short-lived test environments
- Where complexity cost outweighs risk reduction

## Consequences
- Accepted risk: compromise of DC1 = compromise of CA private key
- Mitigated by: CA backup, network segmentation, DC hardening
- Revisit if lab evolves toward production simulation or client-facing scenarios
