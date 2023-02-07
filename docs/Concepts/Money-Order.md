---
title: Money Orders
description: Money Orders
sidebar_position: 3
---

# Money orders

Money orders are used to send transactions between two accounts in a [rollup](Rollups/Overview.md). The transaction process is performed in two steps: the sender creates the money order, then the recipient redeems it. A money order can only be redeemed once stored onchain.

An operator stores money order batches in memory as a sparse Merkle tree (SMT) before sending the batch onchain. An SMT contains the money order batch root hashes that have been added to the rollup.

A money order can be created using the [`createMoneyOrder`](../Reference/JSON-RPC.md#createmoneyorder) API, and redeemed using the [`redeemMoneyOrder`](../Reference/JSON-RPC.md#redeemmoneyorder) API. Both sender and recipient accounts must belong in the rollup.
