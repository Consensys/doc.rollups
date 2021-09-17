---
Description: Describe partially anonymous rollups.
---

# Partially anonymous rollups

Sumo implements partially anonymous rollups which preserve some of the privacy properties of
[anonymous rollups], without requiring users to create zero knowledge proofs, or operators to
generate recursive proofs.

Partial anonymity makes communication between users and operators transparent, allowing operators
to know what transactions they are performing, but transactions are obfuscated on the blockchain.

| Rollup type                    | User and Operator communication | Operator and blockchain communication |
|--------------------------------|---------------------------------|---------------------------------------|
| Standard (transparent) rollups | Transparent                     | Transparent                           |
| Anonymous rollups              | Obfuscated                      | Obfuscated                            |
| Partially anonymous rollups    | Transparent                     | Obfuscated                            |

Partially anonymous rollups use a relatively small state which operators can keep
in memory and easily update, allowing an operator to handle thousands of transactions per second
without impacting performance.

Benefits of partially anonymous rollups over anonymous rollups are:

* Simpler proving schemes.
* Higher transaction throughput due to fewer constraints for user transactions.
* Simpler account management since all account information (except the secret key) can be recovered
    from a user's operator.
* Simplified user experience since users' who have their account information stored with an
    operator need only produce a signature against their account’s public key.

Disadvantages over anonymous rollups are:

* Money orders must be redeemed in their creation order.
* Account activity leaks onchain when updates are made to account hashes.
* The operator performing a transaction can view the full transaction details.
* Participants in a transaction learn their counterparty ID.

!!! information

    [The specification about partially anonymous rollups] contains in-depth information about the rollup
    type.

[anonymous rollups]: https://ethresear.ch/t/account-based-anonymous-rollup/6657
[The specification about partially anonymous rollups]: ../Assets/partially_anonymous_rollups_with_encryption-v1.4-June.pdf
