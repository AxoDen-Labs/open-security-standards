# Coprime-Factor Security Architecture — Blueprint v0.1

## Objective
Engineer defense-in-depth so that layers are **pairwise independent** across crypto, trust roots, codebases, runtimes, supply chains, control planes, and operations. Result: correlated failures collapse; attacker must break **independent** factors.

## Independence Criteria (pairwise)
- **Crypto:** different algorithm families (e.g., AES-GCM ↔ ChaCha20-Poly1305), distinct RNGs and KMS/HSM vendors.  
- **Trust Roots:** separate CAs/HSMs; independent firmware trees.  
- **Codebase:** different libraries/languages/compilers (avoid shared critical deps).  
- **Runtime:** distinct isolation tech (microVM vs containers), ideally different kernels.  
- **Ops:** separate approval flows, credentials, monitoring.  
- **Supply Chain:** distinct CI/CD, artifact signing, registries.  
- **Control Plane:** policy engines/orchestrators cannot impersonate one another.

> If two layers share a catastrophic common dependency, they are **not** coprime.

## Formal Metric
- **Independence Score (IS):** average of independence flags across dimensions for all layer pairs; target **IS ≥ 0.85**.  
- **Common-Mode Risk Index (CMRI):** proportion of shared critical deps; target **≤ 0.1**.

## Reference Stack (example)
- **AuthN:** FIDO2/WebAuthn (P-256, HSM-A) + TOTP (HMAC-SHA1/256, HSM-B) — require **AND** for elevation.  
- **Transport:** TLS1.3 (rustls, AES-GCM) + admin overlay via WireGuard (ChaCha20-Poly1305).  
- **AuthZ:** Dual engines OPA + Cedar with AND-consensus for privileged actions.  
- **Data-at-Rest:** Envelope AES-GCM (KMS-A) + nested ChaCha20-Poly1305 (KMS-B).  
- **Integrity:** Sigstore provenance + internal CA; two independent verifiers.  
- **Runtime:** S1 on microVM/Firecracker; S2 on containers with SELinux/AppArmor.  
- **Audit:** Append-only Merkle log + daily external anchoring.

## Patterns
- **DPE** Dual-Primitive Encryption  
- **DPC** Dual Policy Consensus  
- **SVK** Split-Vendor Key Ceremony  
- **DT** Divergent Transport  
- **TA** Twin Attestation

## Verification
- Red-team single-layer failure drills.  
- Fault-injection (disable one KMS/engine).  
- Synthetic tamper: invalid artifact with correct signature → expect block.  

## Rollout (phased)
1) Baseline IS/CMRI; choose dual primitives/vendors.  
2) Implement DPE, DPC, SVK for one Tier‑1 service; measure.  
3) Extend DT and TA; automate audits.  
4) Org policy: new high‑risk controls must be coprime with existing stack.
