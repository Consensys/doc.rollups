---
description: How to interact with the ConsenSys Rollup smart contract.
---

# Interact with the ConsenSys Rollup smart contract

ConsenSys Rollup provides a smart contract to use [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).
Users can interact with the contract by sending Ethereum to register.

You can view the [partially anonymous rollup contract](https://github.com/ConsenSys/sumo/blob/main/smart_contract/contracts/PartiallyAnonymous.sol).

## Send inbound transfers to smart contract

ETH parameters are: <path-to-rollup.json>

!!! important

    The value amount is in ETH, not Wei.

!!! example "Example ETH inbound transfer"

    ```bash
    export BLOCKCHAIN_NODE="http://localhost:8545"
    node scripts/registerInboundEth.js data/rollup.json ../node-data/test/keys/eth_account_3.acc 0 100.3333
    ```

ERC-20 parameters are: <path-to-rollup.json>

The value amount is in the smallest units.

!!! example "Example ERC-20 inbound transfer"

    ```bash
    export BLOCKCHAIN_NODE="http://localhost:8545"
    node scripts/registerInboundErc20.js data/rollup.json ../node-data/test/keys/eth_account_3.acc 0 0 10000000
    ```

## Send ERC-20 tokens between accounts

!!! example "Example ERC-20 transfer between accounts"

    ```bash
    truffle exec scripts/truffle/transferErc20To.js $(cat data/erc20_addr.addr) $(jq -r .priv_key ../node-data/test/keys/eth_account_10.acc) $(jq -r .address data/rollup.json) 3000
    ```
