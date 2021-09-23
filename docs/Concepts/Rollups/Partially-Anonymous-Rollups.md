---
Description: Describe partially anonymous rollups.
---

# Partially anonymous rollups

A partially anonymous rollup is a type of [rollup](Overview.md) that makes no assumptions about operators' honesty and
achieves [partial anonymity](#partial-anonymity).

Partially anonymous rollups are a type of [zero knowledge rollup (ZK-rollup)].

In partially anonymous rollups, operators must provide a proof of correctness of each batch it creates, in the form of a ZK-SNARK.
Blockchain nodes verify the proof before adding the batch to a block.

## Partial anonymity

Partially anonymous rollups preserve some of the privacy properties of [fully anonymous ZK-rollups (anonymous rollups)],
which obfuscate communication between users and operators, and between operators and the blockchain.

Partial anonymity makes communication between users and operators transparent, allowing operators to know what
transactions they are performing, but transactions are obfuscated on the blockchain.

| ZK-rollup type              | User and operator communication | Operator and blockchain communication |
|-----------------------------|---------------------------------|---------------------------------------|
| Standard ZK-rollups         | Transparent                     | Transparent                           |
| Anonymous rollups           | Obfuscated                      | Obfuscated                            |
| Partially anonymous rollups | Transparent                     | Obfuscated                            |

Partially anonymous rollups use a smaller state which operators can keep in memory and easily update, allowing an
operator to handle thousands of transactions per second without impacting performance.

Benefits of partially anonymous rollups over anonymous rollups are:

* Simpler proving schemes.
* Higher transaction throughput due to fewer constraints for user transactions.
* Simpler account management since all account information (except the secret key) can be recovered from a user's operator.
* Simplified user experience since users' who have their account information stored with an operator need only produce a
  signature against their account’s public key.

Disadvantages over anonymous rollups are:

* [Money orders](../Money-Order.md) must be redeemed in their creation order.
* Account activity leaks onchain when updates are made to account hashes.
* The operator performing a transaction can view the full transaction details.
* Participants in a transaction learn their counterparty ID.

!!! information

    [The specification about partially anonymous rollups] contains in-depth information about the ZK-rollup type.

## Finality

Partially anonymous rollups use the consensus established at the blockchain level.
Finality of partially anonymous rollups equals the blockchain-finality; the rollup's batch is final
once the block that includes it becomes final.

[zero knowledge rollup (ZK-rollup)]: https://ethresear.ch/t/on-chain-scaling-to-potentially-500-tx-sec-through-mass-tx-validation/3477
[fully anonymous ZK-rollups (anonymous rollups)]: https://ethresear.ch/t/account-based-anonymous-rollup/6657
[The specification about partially anonymous rollups]: ../../Assets/partially_anonymous_rollups_with_encryption-v1.4-June.pdf
