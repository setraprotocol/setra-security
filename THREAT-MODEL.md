# Setra Threat Model

## Trust Assumptions

**Trusted**: Governor (multisig), Attesters (bonded), Issuers
**Semi-trusted**: Relayers (may censor/delay), Netting Operators
**Untrusted**: Users, External contracts

## Attack Vectors

### 1. Intent Replay
Nonce-based protection + settled mapping. Residual risk: negligible (keccak256 collision required).

### 2. Front-running
Fixed prices at signing. No MEV in DVP. Residual: low.

### 3. Signature Malleability
OZ ECDSA with s-value normalization. Residual risk: negligible (keccak256 collision required).

### 4. Reentrancy
ReentrancyGuard on all settlement functions. Residual: low.

### 5. Registry Poisoning
Authorized issuers only; governance can deactivate. Residual: medium.

### 6. Netting Manipulation
On-chain tracking; operator is governance multisig. Residual: medium.

### 7. PoR Falsification
Multiple attesters, cooldown, on-chain supply check. Residual: medium.

## Invariants

1. totalSettlements monotonically increases
2. Every settlement has valid EIP-712 sigs from both parties
3. No intent hash settles twice
4. Frozen accounts cannot transfer sEquity
5. Per-account nonce strictly increases
6. Batch size <= 50
7. Only registered active assets settle through DVP
