---
title: Overview
description: ConsenSys Rollups overview
sidebar_position: 1
---

# ConsenSys Rollups

ConsenSys Rollups implements [partially anonymous rollups](Partially-Anonymous-Rollups.md).

A rollup is a *batch* of user transactions.

In a rollup, users create transactions that [interact with the rollup smart contract](../../HowTo/Smart-Contracts.md).
Operators create and manage the rollup offchain.
The batch is published onchain by updating the rollup's state root hash stored in the rollup smart contract.

<!--
When configuring ConsenSys Rollups, you can set the [`rollup_type`](../../Reference/Configuration-File.md#rollup_type)
in the engine configuration file to one of the two types.

## Rollup advantages
-->

ConsenSys Rollups can achieve high throughput, as verification is performed offchain with instant finality.

<!--
Benefits of consensus-rollups over partially anonymous rollups include:

- Higher throughput (~10,000 transactions per second, versus partially anonymous rollups' ~2,000 transactions per second).
- No proof of correctness required.
- Simpler implementation.

Benefits of partially anonymous rollups over consensus-rollups include:

- Transaction anonymity between operators and the blockchain.
- No assumption of operators' honesty.
-->
