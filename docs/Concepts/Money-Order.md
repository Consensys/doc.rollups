---
Description: Money orders
---

# Money orders

Money orders are used to send transactions between two accounts in a rollup. The transaction process
is performed in two steps: the sender creates the money order, then the recipient redeems it. A money order
can only be redeemed once stored onchain.

An Operator stores money order batches in memory as a sparse Merkle tree (SMT) before sending the
batch onchain. An SMT contains the money order batch root hashes that have been added to the rollup.

A money order can be created using the `createMoneyOrder` API, and redeemed using
`redeemMoneyOrder` API. Both sender and recipient accounts must belong in the rollup.
