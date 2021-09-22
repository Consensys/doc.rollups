---
description: How to interact with the ConsenSys Rollup smart contract.
---

# Interact with the rollup smart contract

ConsenSys Rollups provides smart contracts for users to interact with [consensus-rollups](../Concepts/Rollups/Consensus.md)
and [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).
Users can [send inbound transfers to the smart contract](#send-inbound-transfers-to-smart-contract) and
[send money orders](#send-erc-20-money-orders).

You can view the [consensus-rollup smart contract](https://github.com/ConsenSys/sumo/blob/main/smart_contract/contracts/Consensus.sol)
and the [partially anonymous rollup contract](https://github.com/ConsenSys/sumo/blob/main/smart_contract/contracts/PartiallyAnonymous.sol)
in the ConsenSys Rollups repository.

## Send inbound transfers to smart contract

Use [Node.js](https://nodejs.org/en/) to send inbound transfers to the smart contract.

Set the following environment variable to the address of Ethereum RPC:

!!! example

    === "Environment variable"

        ```bash
        export BLOCKCHAIN_NODE=<RPC-address>
        ```

    === "Example"

        ```bash
        export BLOCKCHAIN_NODE=http://localhost:8545
        ```

ETH parameters are: <path-to-rollup.json>

!!! important

    The value amount is in ETH, not Wei.

!!! example "Example ETH inbound transfer"

    ```bash
    node scripts/registerInboundEth.js data/rollup.json ../node-data/test/keys/eth_account_3.acc 0 100.3333
    ```

ERC-20 parameters are: <path-to-rollup.json>

The value amount is in the smallest units.

!!! example "Example ERC-20 inbound transfer"

    ```bash
    node scripts/registerInboundErc20.js data/rollup.json ../node-data/test/keys/eth_account_3.acc 0 0 10000000
    ```

## Send ERC-20 money orders

Use [Truffle](https://www.trufflesuite.com/truffle) to send ERC-20 [money orders](../Concepts/Money-Order.md).

!!! example "Example ERC-20 transfer between accounts"

    ```bash
    truffle exec scripts/truffle/transferErc20To.js $(cat data/erc20_addr.addr) $(jq -r .priv_key ../node-data/test/keys/eth_account_10.acc) $(jq -r .address data/rollup.json) 3000
    ```
