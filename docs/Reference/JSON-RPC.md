---
Description: JSON-RPC API methods
---

# Sumo rollup API methods

## `createAccount`

Creates an account on the rollup. You need to wait for the account to finalized on the blockchain
before creating money orders.

Use [`findAccountId`](#findaccountid) to obtain the account ID and determine
whether the account is ready to use.

### Parameters

* `publicKey`: *string* - 64-byte account public key

* `encryptionKey`: *string* - 64-byte encryption key associated with the supplied public key

* `blindingFactor`: *string* - TODO: what is this, and how is it generated?

* `signature`: *string* - 96-bytes account signature

### Returns

`result`: *string* - status of the account creation, for example `PENDING`, or an error.

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc": "2.0","method": "createAccount", "params": ["0x64001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000","0x64002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000","0x3200000000000000000000000000000000000000000000000000000000000000","0x960000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"],"id": 1}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
          "jsonrpc": "2.0",
          "id": 1,
          "result": {
            "status": "PENDING"
          }
        }
        ```
## createMoneyOrder

Create a money order to transfer an amount between two accounts. Both accounts must be in the
rollup.

### Parameters

* `fromAccountId`: *integer* - 32-byte account ID of the sender

* `toAccountId`: *integer* - 32-byte account ID of the receiver

* `nonce`: *integer* - nonce of the most recent account operation

* `tokenId`: *integer* - token type ID

* `amount`: *integer* - token amount for the transaction

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation

* `signature`: *string* - signature of sender's account

### Returns

`result`: *hash* - money order hash

## findAccountId

Get the ID assigned to an account. You need to determine the ID of an account to create a money
order.

### Parameters

* `publicKey`: *string* - 64-byte account public key

### Returns

`result`: *object* - the ID and status of the account. Status types are `PENDING`, `ACTIVE`, or an
error.

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc": "2.0","method": "findAccountId", "params": ["0x64001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"],"id": 1}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
          "jsonrpc": "2.0",
          "id": 1,
          "result": {
          }
        }
        ```

## getAccountState

Returns the latest finalized account state on the blockchain.

### Parameters

* `accountId`: *integer* - 32-byte account ID

### Returns

`result`: *object* - account state details:

* `id`: *string* - account ID

* `publicKey`: *string* - account public key

* `encryptionKey`: *string* - 64-byte encryption key associated with the public key

* `blindingFactor`: *string* - 32-byte blinding factor

* `nonce`: *integer* - nonce of the most recent account operation

* `redemptionIndex`: *string* -

* `status`: *string* -

* `balances`: *integer* -

* `rootHash`: *string* -

* `merkleProof`: *string* -

## listMoneyOrderCreations

List all money orders for which you are the issuer.

### Parameters

* `nonce`: *integer* - nonce of the most recent account operation

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - 32-byte account ID of the sender

* `toAccountId`: *integer* - 32-byte account ID of the receiver

* `status`: *string* - status of the money order

* `minAmount`: *integer* - filter results by the minimum transaction value

* `maxAmount`: *integer* - filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if more results are available which hasn't been displayed.

* `orders`: *array of objects* - money order creation records:

    * TODO: Find out what the return fields are, and descriptions

## listMoneyOrdersReceived

List money orders available to redeem that are already finalized on the blockchain.

### Parameters

* `fromMoneyOrderTotalIndex`: *integer* - TODO: unsure what this is.

* `nonce`: *integer* - nonce of the most recent account operation

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - 32-byte account ID of the sender

* `toAccountId`: *integer* - 32-byte account ID of the receiver

* `status`: *string* - status of the money order

* `minAmount`: *integer* - filter results by the minimum transaction value

* `maxAmount`: *integer* - filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if more results are available which hasn't been displayed.

* `orders`: *array of objects* - money order redemption records:

    * TODO: Find out what the return fields are, and descriptions

## listMoneyOrdersRedemptions

List money orders submitted for redemption.

### Parameters

* `nonce`: *integer* - nonce of the most recent account operation

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - 32-byte account ID of the sender

* `toAccountId`: *integer* - 32-byte account ID of the receiver

* `status`: *string* - status of the money order

* `minAmount`: *integer* - filter results by the minimum transaction value

* `maxAmount`: *integer* - filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if more results are available which hasn't been displayed.

* `orders`: *array of objects* - money order redemption records:

    * TODO: Find out what the return fields are, and descriptions

## redeemMoneyOrder

Open a money order and claim the funds locked within.

### Parameters

* `fromAccountId`: *integer* - 32-byte account ID of the sender

* `toAccountId`: *integer* - 32-byte account ID of the receiver

* `tokenId`: *integer* - token type ID

* `amount`: *integer* - token amount for the transaction

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation

* `moneyOrderTotalIndex`: *integer* - TODO: unsure what this is.

* `nonce`: *integer* - nonce of the most recent account operation

* `signature`: *string* - signature of sender's account

* `merkleProof`: *string* - Merkle Proof connecting the money order hash to the batch root hash


### Returns

`result`: *object* - money order hash and money order redemption hash
