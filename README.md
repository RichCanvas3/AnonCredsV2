## AnonCreds v2 (BBS+) on Linea — did:aa + Circom

**Privacy-preserving organizational identity & proofs for the EVM.**

AnonCreds-style credentials on Linea with BBS+ selective disclosure and Circom predicates (range, set-membership, equality). Trust is anchored by did:aa (AA smart accounts + ERC-1271). Registries store only hashes and URIs; policy and revocation flow through EAS attestations. UIs are React/TypeScript; proof service is Python (FastAPI).

### Table of contents
- **Overview**
- **Key features**
- **How it works**
- **Quick start (dev)**
- **Roadmap**
- **Security & privacy**
- **Why this exists**

### Overview
- **AnonCreds v2 (BBS+)**: selective-disclosure credentials with unlinkable derived proofs.
- **did:aa trust anchor**: ERC-4337 smart accounts with ERC-1271 verification for issuers/verifiers.
- **Linea-anchored registries**: content hashes + URIs (IPFS/HTTPS) for schemas, cred-defs, and revocation pointers.
- **Attestation rails**: EAS for issuer trust lists, supersession, and revocation updates.
- **Circom predicates**: bound to the same committed attributes as BBS+ proofs.
- **Apps & services**: React/TS UIs (Issuer, Holder, Verifier) and a Python FastAPI proof service.

### Key features
- **Selective disclosure (BBS+)**: prove exactly what’s needed—nothing more.
- **Predicate proofs (Circom)**: age ≥ N, set membership, equality across attributes, etc.
- **Issuer governance (did:aa + ERC-1271)**: contract-controlled DIDs; rotate keys without changing identities.
- **Revocation without leakage**: accumulator/tails off-chain; integrity anchored on Linea.
- **Policy & provenance**: EAS attestations for who/what to trust and what changed when.
- **Interop-friendly**: ledger-agnostic object shapes; pluggable cryptosuites (BBS+ today, PS later).


### How it works (10,000‑ft)
1. **Issuer** (did:aa smart account) publishes schema/cred-def hash + URI to Linea; may emit an EAS `IssuerTrusted` attestation.
2. **Holder** receives a BBS+ credential and derives a selective-disclosure proof and (if requested) a Circom predicate proof bound to the same committed attributes.
3. **Verifier** checks the BBS+ proof and predicate proof off‑chain, then consults Linea registries + attestations for issuer trust and the latest revocation pointer.

### Quick start (dev)
- **Prerequisites**: Node 18+, pnpm, Python 3.11+, circom v2, snarkjs, Foundry/Hardhat, a Linea RPC, and an IPFS pinning option.
- **Deploy**: contracts to Linea Sepolia, then publish a schema and cred-def (hash + URI).
- **Run proofs**: start `services/proofs-py` (FastAPI) for BBS+ derive/verify and Circom predicates.
- **Launch UIs**: run Issuer/Holder/Verifier apps (Vite).

**Note**: No PII is ever written on-chain—only hashes and pointers.


### Security & privacy
- **On-chain minimization**: only hashes/URIs; clients verify content hashes before use.
- **Strong control checks**: ERC-1271 verification; registries gate updates by did:aa.
- **Trusted-setup hygiene**: Groth16 artifacts are versioned and reproducible; circuit changes require new keys.

### Why this exists
Bring AnonCreds‑grade privacy to the agentic, EVM‑native world—with smart‑account governance, gas‑sponsored UX, and composable on‑chain policy—without ever putting sensitive data on-chain.


