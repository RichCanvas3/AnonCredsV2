# AnonCreds v2 (BBS+) on Linea — did:aa + Circom
AnonCreds-style platform on Linea: BBS+ selective disclosure + Circom predicates (range, set, equality) proved off-chain (Python). did:aa (AA) + ERC-1271 govern issuers/verifiers. Registries write only hashes/URIs; EAS attestations for policy &amp; revocation. React/TS UIs.

Privacy-preserving organizational identity & proofs for the EVM.
This project delivers an AnonCreds-style platform built on Linea (Ethereum L2) with did:aa (Account Abstraction smart accounts + ERC-1271) as the trust anchor and BBS+ selective-disclosure proofs. Predicate checks (range, set-membership, equality) are implemented with Circom. ZK proofs run off-chain (Python); the chain is used only for trust, governance, and pointers—never PII.

What this is

AnonCreds v2 (BBS+) credentials with selective disclosure and unlinkable derived proofs.

did:aa issuer/verifier DIDs backed by ERC-4337 smart accounts and ERC-1271 signature validation (governance-ready: multisig, roles, rotations).

Linea-anchored registries that store only content hashes + URIs (IPFS/HTTPS) for schemas, credential definitions, and revocation pointers.

Attestation rails (EAS / Verax) for issuer trust lists, cred-def supersession, and revocation updates.

Circom predicates bound to the same attributes you prove via BBS+ (default: Groth16 commit-then-prove; room left for LegoGroth16 / single-curve composites).

Apps in React/TypeScript for Issuer, Holder, and Verifier; Proof Service in Python (FastAPI).

Key features

Selective disclosure (BBS+): prove exactly what’s needed—nothing more.

Predicate proofs (Circom): age ≥ N, set membership, equality across attributes, etc.

Issuer governance (did:aa + 1271): contract-controlled DIDs; rotate keys without changing identities.

Revocation without leakage: accumulator/tails off-chain; integrity anchored on Linea.

Policy & provenance: EAS/Verax attestations for “who/what to trust” and “what changed when.”

Interop-friendly: ledger-agnostic object shapes; cryptosuite pluggable (BBS+ today, PS later).

Monorepo layout
/contracts          # SchemaRegistry, CredDefRegistry, RevocationRegistry, TrustRegistry
/apps
  issuer-console/   # React + Vite + Viem (+ Delegation Toolkit for AA ops)
  holder-app/
  verifier-console/
/services
  proofs-py/        # FastAPI – BBS+ issue/derive/verify + Circom predicate proofs
/circuits           # Circom predicates (range, set-membership, equality) + keys
/packages
  did-aa/           # TS DID resolver & ERC-1271 helpers
  registries-sdk/   # TS client for registries + EAS/Verax helpers

How it works (10,000-ft)

Issuer (did:aa smart account) publishes schema/cred-def hash + URI to Linea; optionally emits an EAS/Verax IssuerTrusted attestation.

Holder receives a BBS+ credential. On presentation, the Holder derives a selective-disclosure proof and (if requested) a Circom predicate proof bound to the same committed attributes.

Verifier checks the BBS+ proof & predicate proof off-chain, then consults Linea registries + attestations for issuer trust and the latest revocation pointer.

Quick start (dev)

Prereqs: Node 18+, pnpm, Python 3.11+, circom v2, snarkjs, Foundry/Hardhat, a Linea RPC, and an IPFS pinning option.

Deploy: contracts to Linea Sepolia → publish a schema + cred-def (hash + URI).

Run proofs: services/proofs-py (FastAPI) for BBS+ derive/verify + Circom predicates.

Launch UIs: Issuer/Holder/Verifier apps (Vite).

No PII is ever written on-chain—only hashes and pointers.

Roadmap

Composite proofs on BLS12-381 (LegoGroth16) for single-curve elegance.

PS signatures as an alternative cryptosuite (keep APIs stable; flip the suite flag).

Revocation at scale: accumulator ops + watchers; richer attestation schemas.

Aries/Credo adapters for drop-in SSI interop.

Security & privacy

On-chain data is hashes/URIs only; clients verify content hashes before use.

ERC-1271 checks guard issuer/verifier control; registries gate updates by did:aa.

Trusted-setup artifacts (Groth16) are versioned and reproducible; changing circuits requires new keys.

License

Choose what fits your needs (MIT/Apache-2.0 are common); update LICENSE accordingly.

Why this exists: bring AnonCreds-grade privacy to the agentic, EVM-native world—with smart-account governance, gas-sponsored UX, and composable on-chain policy—without ever putting sensitive data on-chain.
