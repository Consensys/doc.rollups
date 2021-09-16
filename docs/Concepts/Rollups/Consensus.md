# Consensus (PoA) rollups

ConsenSys Rollup implements consensus-rollups, or proof of authority (PoA) rollups, which rely on the assumption that
there are enough honest operators to establish a consensus, without relying on a proof of correctness.

In consensus-rollups, operators take turns creating a *batch* of users' transactions (a rollup) to be voted on by the
other operators.

## Finality

Consensus-rollups use the consensus established at the blockchain level.
Finality of consensus-rollups (rollup-finality) is achieved when enough votes finalizing the consensus are included in a
blockchain block.

Rollup-finality does not require a majority of votes.
Rather, the proportion of nodes required to achieve rollup-finality is a configurable threshold, and is a trade-off between
the expected proportion of Byzantine nodes (nodes that may present invalid information) and the target performance.

For example, in a network of 10 nodes:

- If the system is configured to roll back (reject) a batch at 3 rejecting votes, 8 confirming votes are required to
  finalize a batch.
  This enforces safety: if 3 nodes disagree, the system stops.
- If the system is configured to roll back a batch at 8 rejecting votes, 3 confirming votes are required to finalize a batch.
  This makes the system faster and less likely to stop.
  However, if 3 operators are malicious, they can collude to create an invalid rollup that they will accept, forcing the
  7 other validators out.
