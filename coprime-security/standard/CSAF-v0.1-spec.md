# Coprime Security Architecture Framework (CSAF) — v0.1 (Skeleton)

## 1. Scope
This specification defines requirements to claim **Coprime Security** compliance.

## 2. Terminology
- **Layer:** A distinct protection control domain.
- **Independence Dimension:** Crypto, TrustRoot, Codebase, Runtime, Ops, Supply, Control.
- **IS:** Independence Score.  **CMRI:** Common-Mode Risk Index.

## 3. Conformance
- **CSAF‑MUST:** Independence across ≥ 6/7 dimensions for every pair of layers.  
- **CSAF‑SHOULD:** IS ≥ 0.90 for Tier‑1 services.  
- **CSAF‑MUST:** Dual consensus for privileged operations.

## 4. Measurement
Define data schema for independence audits and acceptable tooling outputs.

## 5. Security Considerations
Residual correlation, operational complexity, emergency override governance.
