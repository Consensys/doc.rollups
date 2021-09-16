---
description: Rollups overview
---

# Rollups

ConsenSys Rollup implements two types of rollups:

- [Consensus (PoA) rollups](Consensus.md)
- [Partially anonymous rollups](Partially-Anonymous-Rollups.md)

Rollups can achieve high throughput, as verification is performed off-chain, and instant finality.

Benefits of consensus-rollups over partially anonymous rollups include:

- Higher throughput (~10,000 transactions per second, versus partially anonymous rollups' ~2,000 transactions per second).
- No proof of correctness required.
- Simpler implementation.

Benefits of partially anonymous rollups over consensus-rollups include:

- Transaction anonymity between operators and the blockchain.
- No assumption of operators' honesty.
