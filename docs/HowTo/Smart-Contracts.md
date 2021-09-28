---
description: How to interact with the ConsenSys Rollup smart contract.
---

# Interact with the rollup smart contract

ConsenSys Rollups provides smart contracts for users to interact with [consensus-rollups](../Concepts/Rollups/Consensus.md)
and [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).

The [rollup type specified at configuration](../Reference/Configuration-File.md#rollup_type) determines which contract is used.
The consensus-rollup smart contract and the partially anonymous rollup contract are available in the ConsenSys Rollups repository.

Users can interact with the smart contract by [sending inbound transfers to the smart contract](#send-inbound-transfers-to-smart-contract) to have the smart
contract to manage their tokens, and by [sending tokens between accounts](#send-erc-20-tokens-between-accounts).

ConsenSys Rollups provides accounts with test ether with which you can test the smart contracts.

!!! critical "Security warning"

    **Do not use the test accounts on Ethereum mainnet or any production network.**

    **The test accounts provided in the ConsenSys Rollups repository are not secure; their private keys are publicly
    visible in the source code.**

    Using test accounts on Ethereum mainnet and production networks can lead to loss of funds and identity fraud.

    Always secure your Ethereum mainnet and production accounts properly.
    For example, see [MyCrypto "Protecting Yourself and Your Funds" guide](https://support.mycrypto.com/staying-safe/protecting-yourself-and-your-funds).

## Prerequisites

- ConsenSys Rollups installed and running.
  Navigate to the `smart_contracts` directory to run the example commands.
- [Node.js](https://nodejs.org/en/) installed.
- [Truffle](https://www.trufflesuite.com/truffle) installed.
- Address of the Ethereum RPC set as the following environment variable:

    === "Environment variable"

        ```bash
        export BLOCKCHAIN_NODE=<RPC-address>
        ```

    === "Example"

        ```bash
        export BLOCKCHAIN_NODE=http://localhost:8545
        ```

## Send inbound transfers to smart contract

You can send inbound transfers in [ETH](#in-eth) or [ERC-20 token](#in-erc-20) to the smart contract using the following commands.

### In ETH

=== "Syntax"

    ```bash
    node scripts/registerInboundEth.js data/rollup.json <path-to-Ethereum-account> <account-id-in-rollup> <amount-in-ETH>
    ```

=== "Example using test account"

    ```bash
    node scripts/registerInboundEth.js data/rollup.json ../node-data/test/keys/eth_account_3.acc 0 100.3333
    ```

The transfer amount is in ETH, not Wei.

### In ERC-20

=== "Syntax"

    ```bash
    node scripts/registerInboundErc20.js data/rollup.json <path-to-Ethereum-account> <account-id-in-rollup> <token-id> <transfer-value>
    ```

=== "Example using test account"

    ```bash
    node scripts/registerInboundErc20.js data/rollup.json ../node-data/test/keys/eth_account_3.acc 0 0 10000000
    ```

The transfer amount is in the smallest unit of the ERC-20 token.

## Send ERC-20 tokens between accounts

You can send ERC-20 tokens between accounts using the following command.

=== "Syntax"

    ```bash
    truffle exec scripts/truffle/transferErc20To.js <erc20-address> <priv-key> <to> <amount>
    ```

=== "Example using test account"

    ```bash
    truffle exec scripts/truffle/transferErc20To.js $(cat data/erc20_addr.addr) $(jq -r .priv_key ../node-data/test/keys/eth_account_10.acc) $(jq -r .address data/rollup.json) 3000
    ```

The transfer amount is in the smallest unit of the ERC-20 token.
