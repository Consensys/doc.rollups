---
description: ZK rollups concepts page.
---

# ZK-rollups

ConsenSys Rollup implements [zero-knowledge rollups](https://ethresear.ch/t/on-chain-scaling-to-potentially-500-tx-sec-through-mass-tx-validation/3477),
or ZK-rollups, which make no assumptions about operators' honesty.

In ZK-rollups, operators create a *batch* of transactions (a rollup), and must provide a proof of correctness of the batch
in the form of a ZK-SNARK.
Blockchain nodes verify the proof before adding the ZK-rollup to a block.

Advantages of ZK-rollups include:

- High throughput, as verification is performed off-chain.
- Instant finality.
- Zero trust assumptions; ZK-rollups support a single untrusted operator.

## Finality

ZK-rollups use the consensus established at the blockchain level.
Finality of ZK-rollups (rollup-finality) equals the blockchain-finality: the ZK-rollup's batch is final once the block
that includes it becomes final.
