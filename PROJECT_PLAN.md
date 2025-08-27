## Project Plan — Phased plan and Definition of Done (DoD)

A structured roadmap to deliver AnonCreds v2 (BBS+) on Linea, organized into phases with clear outcomes.

### Table of contents
- Phase 0 — Decisions & env
- Phase 1 — did:aa + ERC-1271
- Phase 2 — Registries + attestations
- Phase 3 — Proof Service (Python, BBS+)
- Phase 4 — Circom predicates
- Phase 5 — Revocation (privacy-preserving)
- Phase 6 — Apps UX polish
- Phase 7 — Interop & parity

---

### Phase 0 — Decisions & env
- Establish binding approach: Groth16-bn128 commit-then-prove.
- Stand up Linea (Mainnet 59144, Sepolia 59141) RPC + deploy tooling; wire EAS SDKs.
- **DoD**: scripts connect; a demo attestation posts on Linea Sepolia.

References:
- `Linea`
- `ChainList`

---

### Phase 1 — did:aa + ERC-1271
- Publish a minimal did:aa method & resolver; deploy AA smart account; prove control with `isValidSignature`.
- **DoD**: Verifier console resolves did:aa and validates ERC-1271 on Linea (via Viem/Delegation Toolkit).

References:
- `docs.metamask.io`

---

### Phase 2 — Registries + attestations
- Solidity: `SchemaRegistry`, `CredDefRegistry`, `RevocationRegistry`, `TrustRegistry` (reads EAS).
- **DoD**: Issuer Console writes schema & cred-def hash+URI under did:aa; TrustRegistry displays `IssuerTrusted`.

References:
- `GitHub`
- `Linea`

---

### Phase 3 — Proof Service (Python, BBS+)
- Integrate `ursa-bbs-signatures` (or Rust FFI) for keygen/sign/derive/verify on BLS12-381.
- REST endpoints: `/issue`, `/derive`, `/verify`, `/verify-with-predicate`.
- **DoD**: SD proof verifies; basic non-revocation stub in place.

References:
- `PyPI`

---

### Phase 4 — Circom predicates
- Implement circuits: range, set-membership, equality; generate proving/verification keys.
- Implement binding:
  - Groth16 (bn128) + commitment consistency.
- **DoD**: “age ≥ 21” + “country disclosed” demo succeeds (BBS+ SD + Circom).

References:
- `Dock Labs Blog`
- `docs.circom.io`

---

### Phase 5 — Revocation (privacy-preserving)
- Off-chain accumulator & tails generation; publish commitment hash + URI on Linea; optional `RevocationUpdated` attestation.
- **DoD**: Verifier fetches latest pointer, checks hash integrity, runs non-revocation in Python.

References:
- `lf-hyperledger.atlassian.net`

---

### Phase 6 — Apps UX polish
- Issuer: create schema/cred-def → publish → attestation.
- Holder: receive BBS+ cred → derive proof (+ optional predicate) via Python.
- Verifier: policy (allowlist, min schema ver) → check BBS+, check predicate, read Linea registries + EAS.
- **DoD**: full E2E demo (Linea readbacks visible on explorer).

References:
- `Linea`

---

### Phase 7 — Interop & parity
- Keep object shapes close to AnonCreds v2; document mapping; test against anoncreds-v2-rs samples when available.
- **DoD**: import/export round-trips with v2-style objects.

References:
- `GitHub`

