---
Description: JSON-RPC API methods
---

# ConsenSys Rollups API methods

## pazkp_createAccount

Creates an account on the rollup.

!!! tip

    You need to wait for the account to finalized on the blockchain before creating money orders.

    Use [`pazkp_findAccountByPublicKey`](#pazkp_findAccountByPublicKey) to obtain the account ID and determine whether the account is ready to use.

### Parameters

* `publicKey`: *string* - 64-byte account public key

* `jointEncryptionKey`: *string* - 128-byte encryption key based on the user's public key and
    operator's public key assembled using the ConsenSys Rollups cryptographic library

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation

* `encryptedBlindingFactor`: *string* - blinding factor encrypted using `jointEncryptionKey`

* `randomizationFactor`: *string* - value produced during encryption used to check whether data has been
    encrypted correctly

* `signature`: *string* - 96-byte account signature

### Returns

`result`: *object* - account creation details

* `status`: *string* - account creation status, for example `REGISTERED`; otherwise an error, for example
`PUBLIC_KEY_ALREADY_EXISTS`.

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_createAccount","params":["0x0ec2cd6bb8213dbe3de41dc828f54a5cf2cc8127533899d70ad8cb6d915efd97167698318dd99feeeb6ff9c2b06bc85fa2f22c596f3782b63d99bd6553d34542","0x018bfd3d600db9f51f5e69d30144051c22f624b96455c1ac891362d3dd9c265b21d476dc08d1a79de4b73b8fce6f9c3bd2f6a195e2b363bcd7e069f439f56443018bfd3d600db9f51f5e69d30144051c22f624b96455c1ac891362d3dd9c265b21d476dc08d1a79de4b73b8fce6f9c3bd2f6a195e2b363bcd7e069f439f56443","0x0d5c179267be7695e3aeef4249b4bb845e904d188c64fc07b41c5bb4b529db03","0x0000000000000003024362050bdb3c6f511b07639ff6425009fc6b7665d34966a71790c38de7ba2c2325ead882e7ef4a174c63d3f03844296844177e885d6969980d0a354f7922462a3d4299f0053fd6859a1279813bf008c1920da7de9f9c757b9f56f793cb611b113430f9039e3018985cefac7683010c2f520f52621f171a6a097f7e0fc863e72a3d4299f0053fd6859a1279813bf008c1920da7de9f9c757b9f56f793cb611b113430f9039e3018985cefac7683010c2f520f52621f171a6a097f7e0fc863e700000000000000012854d1f8a4c50d92e2b66e21636d55059ee6b812bac2a751e1254ad72d7315e9","0x0b94aeab3040a0e7f5670d413751fe0c38ec52b2e5ffee938f07c16765c08559","0x1efe34e4c66965b07c80547e420903f6e143763b36e95090c3e4a4e55a354b761ce3628ab45abe232b742058ad47a7b694efbd0e791601954a1dd2017f67b9a60347d80317ac0191f79e7c5780de632d92c2c1a7772a1db48d343e9639ad34f2"],"id":0}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 0,
            "result": {
                "status": "REGISTERED"
            }
        }
        ```

## pazkp_createMoneyOrder

Create a money order to transfer an amount between two accounts. Both accounts must be in the rollup.

### Parameters

* `fromAccountId`: *integer* - 32-bit account ID of the sender

* `toAccountId`: *integer* - 32-bit account ID of the receiver

* `tokenId`: *integer* - token type ID

* `amount`: *integer* - token amount for the transaction

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation

* `encryptedMoneyOrder`: *string* - money order redemption encrypted using the receiver's `jointEncryptionKey`

* `randomizationFactor`: *string* - value produced during encryption used to check whether data has been
    encrypted correctly

* `nonce`: *integer* - nonce of the most recent account operation

* `signature`: *string* - signature of sender's account

### Returns

`result`: *object* - money order creation details

* `operationHash`: *string* - hash of the operation

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_createMoneyOrder","params":[0,1,0,100,"0x22879dd7ce3e297b879a52225026f256d8758bb700c40d53b769472218cfe740","0x000000000000000510c380d46202de6b88ccac3a89918f5a64691fc6ab638e200d81687856413bea304d894c75e86a57d7cf237d4eb6bd89036ef8db6b7fadcd8a274cce3cb3cea40edec9cf26b591e617223be7737d4851583772a34107c5e8063868731a56b0dc1bea0fa146e057a9ab260f58cd3364e1e1dd1c939a45e38b948801ca778bf5890edec9cf26b591e617223be7737d4851583772a34107c5e8063868731a56b0dc1bea0fa146e057a9ab260f58cd3364e1e1dd1c939a45e38b948801ca778bf5890edec9cf26b591e617223be7737d4851583772a34107c5e8063868731a56b0dc1bea0fa146e057a9ab260f58cd3364e1e1dd1c939a45e38b948801ca778bf5890edec9cf26b591e617223be7737d4851583772a34107c5e8063868731a56b0dc1bea0fa146e057a9ab260f58cd3364e1e1dd1c939a45e38b948801ca778bf58900000000000000020d31b221072da2c8d0617ed83c97d1a23121ba670f3a907e7025cdc639aac2a60c55f160c2fd8a9308c494ab4405605554cae3b9fb0ee451928e85dc2e9b1974","0x104e6481626a5faaf4bc1491270ba50d131ff8f1c40b981d152f89e3cfd167b9",1,"0x1a79b79570157ee66af363c838018faa920c0b2e035091902ceff2963c1766b42082c4bb5f7641ea90d196406e45b1548a91ac4eaa580bb554a88e2ee36d4e8a03a50ef82c307aa57caf90d5fbf231c3c7e9242e7da2c2bffe5b13f3575ec653"],"id":30}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 30,
            "result": {
                "operationHash": "0x24b2e10dade2ef8e3732003490121c1f0ef9b174095a2318ef2fa8cad9dc9885"
            }
        }
        ```

## pazkp_createOutboundTransfer

Create an outbound transfer to move an amount between a rollup account and an Ethereum address.

### Parameters

* `fromAccountId`: *integer* - 32-bit account ID of the sender

* `toEthereumAddress`: *integer* - 20-byte Ethereum address of the receiver

* `tokenId`: *integer* - token type ID

* `amount`: *integer* - token amount for the transaction

* `nonce`: *integer* - nonce of transaction

* `signature`: *string* - signature of sender's account

### Returns

`result`: *object* - outbound transfer details

* `operationHash`: *string* - hash of the operation

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_createOutboundTransfer","params":[1,"0x0f38616f3f7ca7316aceea7adb3fd53fa2527026",1,5,5,"0x28cc754c9587098274ff9936b3496b6d00bba7eab79bbb72b50804d5a1a5715b2cddca0d2a25e0d203e9d0a1b1fb2c0952d7ffc28affb843b6b36ffd37d863ad01d9c94ca421176eca05f2bee506a913a7040ee120613f18127d80262f74c751"],"id":89}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 89,
            "result": {
                "operationHash": "0x1780f941d14acb15d4c906c6b1b54ba77d9d4d6558079183b25f8731424ee498"
            }
        }
        ```

## pazkp_findAccountById

Get the public details of an account.

### Parameters

* `accountId`: *integer* - account ID

### Returns

`result`: *object* - account details

* `id`: *integer* - account ID

* `publicKey`: *string* - 64-byte account public key

* `jointEncryptionKey`: *string* - 128-byte encryption key based on the user's public key and
  operator's public key assembled using the ConsenSys Rollups cryptographic library

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_findAccountById","params":[0],"id":10}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 10,
            "result": {
                "account": {
                    "id": 0,
                    "publicKey": "0x0ec2cd6bb8213dbe3de41dc828f54a5cf2cc8127533899d70ad8cb6d915efd97167698318dd99feeeb6ff9c2b06bc85fa2f22c596f3782b63d99bd6553d34542",
                    "jointEncryptionKey": "0x018bfd3d600db9f51f5e69d30144051c22f624b96455c1ac891362d3dd9c265b21d476dc08d1a79de4b73b8fce6f9c3bd2f6a195e2b363bcd7e069f439f56443018bfd3d600db9f51f5e69d30144051c22f624b96455c1ac891362d3dd9c265b21d476dc08d1a79de4b73b8fce6f9c3bd2f6a195e2b363bcd7e069f439f56443"
                }
            }
        }
        ```

## pazkp_findAccountByPublicKey

Get the public details of an account, including the account ID. You need to determine the ID of an account to create a money order.

### Parameters

* `publicKey`: *string* - 64-byte account public key

### Returns

`result`: *object* - account details

* `id`: *integer* - account ID

* `publicKey`: *string* - 64-byte account public key

* `jointEncryptionKey`: *string* - 128-byte encryption key based on the user's public key and
  operator's public key assembled using the ConsenSys Rollups cryptographic library

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_findAccountByPublicKey","params":["0x0ec2cd6bb8213dbe3de41dc828f54a5cf2cc8127533899d70ad8cb6d915efd97167698318dd99feeeb6ff9c2b06bc85fa2f22c596f3782b63d99bd6553d34542"],"id":26}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 26,
            "result": {
                "account": {
                    "id": 0,
                    "publicKey": "0x0ec2cd6bb8213dbe3de41dc828f54a5cf2cc8127533899d70ad8cb6d915efd97167698318dd99feeeb6ff9c2b06bc85fa2f22c596f3782b63d99bd6553d34542",
                    "jointEncryptionKey": "0x018bfd3d600db9f51f5e69d30144051c22f624b96455c1ac891362d3dd9c265b21d476dc08d1a79de4b73b8fce6f9c3bd2f6a195e2b363bcd7e069f439f56443018bfd3d600db9f51f5e69d30144051c22f624b96455c1ac891362d3dd9c265b21d476dc08d1a79de4b73b8fce6f9c3bd2f6a195e2b363bcd7e069f439f56443"
                }
            }
        }
        ```

## pazkp_findMoneyOrderCreation

Find a specific money order for which you are the issuer.

### Parameters

* `operationHash`: *integer* - hash of the operation

### Returns

`result`: *object* - money order creation details

* `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
  `blindingFactor`

* `nonce`: *integer* - transaction nonce

* `status`: *string* - status of the money order, for example `PENDING` or `EXECUTED`

* `rejectionReason`: *string* - rejection reason details, when status of the money order is `REJECTED`

* `operationHash`: *string* - hash of the operation

* `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
        money order batch, and 2 bytes are for the index of the money order in the batch.

* `batchHeight`: *integer* - the unique batch index in the rollup that contains the transaction, following ascending ordinal numbering

* `batchStateRootHash`: *string* - rollup `rootHash` that corresponds to the batch in which the money order
  was included

* `blockNumber`: *integer* - the Ethereum block number in which the batch was included in the rollup smart contract

* `blockHash`: *string* - the Ethereum block hash in which the batch was included

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_findMoneyOrderCreation","params":[0,"0x24b2e10dade2ef8e3732003490121c1f0ef9b174095a2318ef2fa8cad9dc9885"],"id":128}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 128,
            "result": {
                "moneyOrder": {
                    "fromAccountId": 0,
                    "toAccountId": 1,
                    "tokenId": 0,
                    "amount": 100,
                    "blindingFactor": "0x22879dd7ce3e297b879a52225026f256d8758bb700c40d53b769472218cfe740"
                },
                "nonce": 1,
                "status": "EXECUTED",
                "rejectionReason": null,
                "operationHash": "0x24b2e10dade2ef8e3732003490121c1f0ef9b174095a2318ef2fa8cad9dc9885",
                "moneyOrderTotalIndex": 65536,
                "batchHeight": 2,
                "batchStateRootHash": "0x03d7a6c15020157656479946bf06eba93f5a7d0260a5b3931729dfa62f45a6e9",
                "blockNumber": 8280,
                "blockHash": "0x5b765d42462657b4b887df110df76789c6bccd7ce4a1c324a13bf71d985e496e"
            }
        }
        ```

## pazkp_findMoneyOrderRedemption

Find a specific money order submitted for redemption.

### Parameters

* `operationHash`: *integer* - hash of the operation

### Returns

`result`: *object* - money order redemption details

* `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
  `blindingFactor`

* `nonce`: *integer* - transaction nonce

* `status`: *string* - status of the money order, for example `PENDING` or `EXECUTED`

* `rejectionReason`: *string* - rejection reason details, when status of the money order is `REJECTED`

* `operationHash`: *string* - hash of the operation

* `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
        money order batch, and 2 bytes are for the index of the money order in the batch.

* `merkleProof`: *array* - merkle proof of the account in the merkle tree

* `batchHeight`: *integer* - the unique batch index in the rollup that contains the transaction, following ascending ordinal numbering

* `batchStateRootHash`: *string* - rollup `rootHash` that corresponds to the batch in which the money order
  was included

* `blockNumber`: *integer* - the Ethereum block number in which the batch was included in the rollup smart contract

* `blockHash`: *string* - the Ethereum block hash in which the batch was included

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_findMoneyOrderRedemption","params":[1,"0x105cbc9a6c55a5a525c81993a1a3f6c0d0b9786a38a76b63f5e6652423c946f2"],"id":70}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 70,
            "result": {
                "moneyOrder": {
                    "fromAccountId": 0,
                    "toAccountId": 1,
                    "tokenId": 0,
                    "amount": 100,
                    "blindingFactor": "0x22879dd7ce3e297b879a52225026f256d8758bb700c40d53b769472218cfe740"
                },
                "nonce": 1,
                "status": "EXECUTED",
                "rejectionReason": null,
                "operationHash": "0x105cbc9a6c55a5a525c81993a1a3f6c0d0b9786a38a76b63f5e6652423c946f2",
                "moneyOrderTotalIndex": 65536,
                "merkleProof": [
                    "0x0000000000000000000000000000000000000000000000000000000000000000",
                    "0x0e1008befc9615fb581a54efdf52d6ddbc172b73fe5d8a899f505733cc22af3c",
                    "0x12022ccdddb84346316e8c6e4c92cf903e53f61233c9ee2ed8bc707da363ae76",
                    "0x180b25b21a023057c36731b4f58320328ce7695889a427327af20f23e3f96559",
                    "0x079a2fd63656ffa10c3ff2f666b0dc7ad40618053f3b614d6b9d4bb2be632ae2",
                    "0x2e6fd101fc2f2556d5e2aef1b7ee77bd644677c1c98ecfd77835ba3b927753c8",
                    "0x07ff3bfea803efd5bf5a6b319ba499d4bb01baebde1a648d3e1a49f36417a1a8",
                    "0x064cb863bce8fc4f5e958dc3e14519ffb7dc064e9556fda668df5a3fb9bcfa6a",
                    "0x11bea956b00a53cc839a6bf92641de1c193196b31d60dad105d4107562de9b7e",
                    "0x1beb350a107c06bfb85d1f54b6f4792d374405e07b4fe388d3e1f6e907ce97ae",
                    "0x221b38aa9fa947da92a67def48ab4acefd652ab7ba82c86485f214f5e341e6a1",
                    "0x133b231a8c5584c0495416312b10b453dcdc05cdce899541a38dc54b87448908"
                ],
                "batchHeight": 5,
                "batchStateRootHash": "0x2328c9b8173a6e3b37b37ca91ec156cd2737b6e597240c2e62f27ae3724359a8",
                "blockNumber": 8290,
                "blockHash": "0xc15c3a87508aeee55b3d7ca9c2f2ef0501156b27f6de921d9841a85ab642734d"
            }
        }
        ```

## pazkp_findOutboundTransfer

Find a specific outbound transfer.

### Parameters

* `operationHash`: *integer* - hash of the operation

### Returns

`result`: *object* - money order redemption details

* `fromAccountId`: *integer* - 32-bit account ID of the sender

* `toEthereumAddress`: *string* - 20-byte Ethereum address of the receiver

* `tokenId`: *integer* - token type ID

* `amount`: *integer* - token amount for the transaction

* `status`: *string* - status of the money order, for example `PENDING` or `EXECUTED`

* `rejectionReason`: *string* - rejection reason details, when status of the money order is `REJECTED`

* `operationHash`: *string* - hash of the operation

* `batchHeight`: *integer* - the unique batch index in the rollup that contains the transaction, following ascending ordinal numbering

* `batchStateRootHash`: *string* - rollup `rootHash` that corresponds to the batch in which the money order
  was included

* `blockNumber`: *integer* - the Ethereum block number in which the batch was included in the rollup smart contract

* `blockHash`: *string* - the Ethereum block hash in which the batch was included

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_findOutboundTransfer","params":[1,"0x12fb7a82834dfcb8c8e4c82b34744c92abbf74e3f53106590fe55025c4578e57"],"id":98}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 98,
            "result": {
                "fromAccountId": 1,
                "toEthereumAddress": "0x0f38616f3f7ca7316aceea7adb3fd53fa2527026",
                "tokenId": 1,
                "amount": 5,
                "nonce": 6,
                "status": "EXECUTED",
                "rejectionReason": null,
                "operationHash": "0x12fb7a82834dfcb8c8e4c82b34744c92abbf74e3f53106590fe55025c4578e57",
                "batchHeight": 8,
                "batchStateRootHash": "0x0e71ca8229fb238f5db496e492b1353c7bb6d0ccd058a63847dfe76822c2923f",
                "blockNumber": 8300,
                "blockHash": "0x039cba9c210e5e9229dce54c57a27663c0b46d08b1f19f44ad6bc3a6ba8f8b8c"
            }
        }
        ```

## pazkp_findReceivedMoneyOrder

Find a specific money order that is either available to redeem or has been redeemed.

### Parameters

* `accountId`: *integer* - account ID

* `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
        money order batch, and 2 bytes are for the index of the money order in the batch.

### Returns

`result`: *object* - received money order details

* `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
  `blindingFactor`

* `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
        money order batch, and 2 bytes are for the index of the money order in the batch.

* `merkleProof`: *array* - merkle proof of the account in the merkle tree

* `batchHeight`: *integer* - the unique batch index in the rollup that contains the transaction, following ascending ordinal numbering

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_findReceivedMoneyOrder","params":[1,65536],"id":121}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 121,
            "result": {
                "moneyOrder": {
                    "fromAccountId": 0,
                    "toAccountId": 1,
                    "tokenId": 0,
                    "amount": 100,
                    "blindingFactor": "0x22879dd7ce3e297b879a52225026f256d8758bb700c40d53b769472218cfe740"
                },
                "moneyOrderTotalIndex": 65536,
                "merkleProof": [
                    "0x0000000000000000000000000000000000000000000000000000000000000000",
                    "0x0e1008befc9615fb581a54efdf52d6ddbc172b73fe5d8a899f505733cc22af3c",
                    "0x12022ccdddb84346316e8c6e4c92cf903e53f61233c9ee2ed8bc707da363ae76",
                    "0x180b25b21a023057c36731b4f58320328ce7695889a427327af20f23e3f96559",
                    "0x079a2fd63656ffa10c3ff2f666b0dc7ad40618053f3b614d6b9d4bb2be632ae2",
                    "0x2e6fd101fc2f2556d5e2aef1b7ee77bd644677c1c98ecfd77835ba3b927753c8",
                    "0x07ff3bfea803efd5bf5a6b319ba499d4bb01baebde1a648d3e1a49f36417a1a8",
                    "0x064cb863bce8fc4f5e958dc3e14519ffb7dc064e9556fda668df5a3fb9bcfa6a",
                    "0x11bea956b00a53cc839a6bf92641de1c193196b31d60dad105d4107562de9b7e",
                    "0x1beb350a107c06bfb85d1f54b6f4792d374405e07b4fe388d3e1f6e907ce97ae",
                    "0x221b38aa9fa947da92a67def48ab4acefd652ab7ba82c86485f214f5e341e6a1",
                    "0x133b231a8c5584c0495416312b10b453dcdc05cdce899541a38dc54b87448908"
                ],
                "batchHeight": 2
            }
        }
        ```

## pazkp_getAccountNonce

Returns the most recent nonce and redemption index for the account.

### Parameters

* `accountId`: *integer* - account ID

### Returns

`result`: *object* - account state details:

* `id`: *string* - account ID

* `nonce`: *integer* - nonce of the most recent account operation

* `redemptionIndex`: *string* - redemption index of the most recent redeemed money order

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_getAccountNonce","params":[0],"id":6}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 22,
            "result": {
                "id": 6,
                "nonce": 0,
                "redemptionIndex": 0
            }
        }
        ```

## pazkp_getAccountState

Returns the latest finalized account state on the blockchain.

### Parameters

* `accountId`: *integer* - account ID

### Returns

`result`: *object* - account state details:

* `id`: *string* - account ID

* `publicKey`: *string* - account public key

* `jointEncryptionKey`: *string* - 128-byte encryption key based on the user's public key and
    operator's public key assembled using the ConsenSys Rollups cryptographic library

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation

* `nonce`: *integer* - nonce of the most recent account operation

* `redemptionIndex`: *string* - redemption index of the most recent redeemed money order

* `balances`: *object* - a map of the token balance for each token index.

* `rootHash`: *string* - root hash of the account in the merkle tree

* `merkleProof`: *array* - merkle proof of the account in the merkle tree

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_getAccountState","params":[0],"id":27}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 27,
            "result": {
                "id": 0,
                "publicKey": "0x0ec2cd6bb8213dbe3de41dc828f54a5cf2cc8127533899d70ad8cb6d915efd97167698318dd99feeeb6ff9c2b06bc85fa2f22c596f3782b63d99bd6553d34542",
                "jointEncryptionKey": "0x018bfd3d600db9f51f5e69d30144051c22f624b96455c1ac891362d3dd9c265b21d476dc08d1a79de4b73b8fce6f9c3bd2f6a195e2b363bcd7e069f439f56443018bfd3d600db9f51f5e69d30144051c22f624b96455c1ac891362d3dd9c265b21d476dc08d1a79de4b73b8fce6f9c3bd2f6a195e2b363bcd7e069f439f56443",
                "blindingFactor": "0x0d5c179267be7695e3aeef4249b4bb845e904d188c64fc07b41c5bb4b529db03",
                "nonce": 0,
                "redemptionIndex": 0,
                "balances": {
                    "1": 1000000000,
                    "0": 1000000000
                },
                "rootHash": "0x2cadf3d430a64c373c4fa685b7f34907a7bd0d5823f678d78dd1d11ea3e4eba8",
                "merkleProof": [
                    "0x2a93fd92709007d86835ad933d80219207b8d10f1449853cd902489d63a30c2c",
                    "0x0e1008befc9615fb581a54efdf52d6ddbc172b73fe5d8a899f505733cc22af3c",
                    "0x12022ccdddb84346316e8c6e4c92cf903e53f61233c9ee2ed8bc707da363ae76",
                    "0x180b25b21a023057c36731b4f58320328ce7695889a427327af20f23e3f96559",
                    "0x079a2fd63656ffa10c3ff2f666b0dc7ad40618053f3b614d6b9d4bb2be632ae2",
                    "0x2e6fd101fc2f2556d5e2aef1b7ee77bd644677c1c98ecfd77835ba3b927753c8",
                    "0x07ff3bfea803efd5bf5a6b319ba499d4bb01baebde1a648d3e1a49f36417a1a8",
                    "0x064cb863bce8fc4f5e958dc3e14519ffb7dc064e9556fda668df5a3fb9bcfa6a",
                    "0x11bea956b00a53cc839a6bf92641de1c193196b31d60dad105d4107562de9b7e",
                    "0x1beb350a107c06bfb85d1f54b6f4792d374405e07b4fe388d3e1f6e907ce97ae",
                    "0x221b38aa9fa947da92a67def48ab4acefd652ab7ba82c86485f214f5e341e6a1",
                    "0x133b231a8c5584c0495416312b10b453dcdc05cdce899541a38dc54b87448908",
                    "0x2aaf888cb0d9d6140d2de35479a8255c74dcec9b484e6499c0f8645eb3ca4420",
                    "0x1fcd7436d4f32922a4a401eca664fadfdc1d78a33167b91661f6f70d11d7ee8a",
                    "0x0500632004a6f045435113ee2d538c5a9b9c3fc6d5efbb9759c168fc2e827935",
                    "0x2f3766dff35af63b997a22de0030267f7a744b9d54613ee6093b3b16736feab2",
                    "0x11f93c31ad92b36d9943dd3aa319e4eef601da4dbd62d47e40ceabc300ba5651",
                    "0x16a2a7ab96b615cf38ddaf38e72cbbd0e739c4d54dbd731b1a2a23d2e1d461f6",
                    "0x157243b1204b6be441716fc3fb75d940f46d34c782cd232480f2957e2a97869d",
                    "0x1855705ee69c123fffafee53616b35055de6426e948acd7bedb747b1940f65b1",
                    "0x301d77e4a08447e0ed153b00a696ee487fe4799b0d037e5c2f1de97f76cc28d9",
                    "0x257f56cc770b963d39516e846ede2e82857931082e4f32ffa4b0e96d97e711ae",
                    "0x1f4fcdc26f3fc9758475b5cd03b4cfe8270a54c2b92fc4cba95368bcf3b6dcf3",
                    "0x086f593ae83a4ef5d3a72217d8200fe7029bc1f5a8bb36214bf65886780bc84e",
                    "0x10f2bc44b72996b11ceca74d80a2bd308d8c8f8c99945d9e062353ea43e4d998",
                    "0x0da457fa81a7359277182b3e89912c66fdc25445002b2b1cbf44639b6e4a96fa",
                    "0x1c9d5ef2524f748a1bb37b0200d98de35a1f7e3bdefdfb350a101d090d46e10d",
                    "0x1fac134b65a988f706d96c21ff57db1bc3ce3222c76417db93af07a12fcc5b7a",
                    "0x203c283f5890710b4a28ad3c4a28f8aa702abe36f46f9c0c0d858419f57b285b"
                ]
            }
        }
        ```

## pazkp_listMoneyOrderCreations

List all money orders for which you are the issuer.

### Parameters

!!! important

    Use `null` if you don't want to specify a value for an optional parameter.

* `nonce`: *integer* - transaction nonce

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - 32-bit account ID of the sender

* `toAccountId`: *integer* - (optional) 32-bit account ID of the receiver

* `status`: *string* - (optional) status of the money order

* `minAmount`: *integer* - (optional) filter results by the minimum transaction value

* `maxAmount`: *integer* - (optional) filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if there are more results available, which haven't been displayed

* `items`: *array of objects* - money order creation records:

    * `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
        `blindingFactor`

    * `nonce`: *integer* - transaction nonce

    * `status`: *string* - status of the money order, for example `PENDING` or `EXECUTED`

    * `rejectionReason`: *string* - rejection reason details, when status of the money order is `REJECTED`

    * `operationHash`: *string* - hash of the operation

    * `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
        money order batch, and 2 bytes are for the index of the money order in the batch.

    * `batchHeight`: *integer* - the unique batch index in the rollup that contains the transaction, following ascending ordinal numbering

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_listMoneyOrderCreations","params":[0,10,0,null,null,null,null],"id":107}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 107,
            "result": {
                "hasMoreResults": false,
                "items": [
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 0,
                            "amount": 100,
                            "blindingFactor": "0x22879dd7ce3e297b879a52225026f256d8758bb700c40d53b769472218cfe740"
                        },
                        "nonce": 1,
                        "status": "EXECUTED",
                        "operationHash": "0x24b2e10dade2ef8e3732003490121c1f0ef9b174095a2318ef2fa8cad9dc9885",
                        "moneyOrderTotalIndex": 65536,
                        "batchHeight": 2
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 0,
                            "amount": 100,
                            "blindingFactor": "0x22879dd7ce3e297b879a52225026f256d8758bb700c40d53b769472218cfe740"
                        },
                        "nonce": 1,
                        "status": "REJECTED",
                        "rejectionReason": "NONCE_TOO_LOW",
                        "operationHash": "0x1765adad3b23f2d9a659c1253c5984aec54121a19b37e0e67fbb929f5d09d271"
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 1,
                            "amount": 50,
                            "blindingFactor": "0x204f2eff2a733beb0bf490ee3e60ad108cef8f87e7d2a9a5e0f36dc8888eb295"
                        },
                        "nonce": 2,
                        "status": "EXECUTED",
                        "operationHash": "0x241eb61425c39c40ac03490ebe3f8203b5517967233e9d545d3bc7174002c374",
                        "moneyOrderTotalIndex": 131072,
                        "batchHeight": 3
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 0,
                            "amount": 200,
                            "blindingFactor": "0x03df039c5641ed40cd9eddc5e2a81e3fa18b38e32ddfa5912cb57d7b94cbfdb6"
                        },
                        "nonce": 3,
                        "status": "EXECUTED",
                        "operationHash": "0x18d6125b922e5aee7648b29a23b3518b5fce8003cfb4a9f3b0c2826bf62f0a9c",
                        "moneyOrderTotalIndex": 131073,
                        "batchHeight": 3
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 1,
                            "amount": 50,
                            "blindingFactor": "0x193d2c8d8fd20908f844a4d8b18cd07b4450710c49f6d2e12a62542625ff95e3"
                        },
                        "nonce": 4,
                        "status": "EXECUTED",
                        "operationHash": "0x2121dd349a82524b1e27b37e044b569efeed0254f70ee838aace3bbf2888006d",
                        "moneyOrderTotalIndex": 196608,
                        "batchHeight": 4
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 2,
                            "amount": 50,
                            "blindingFactor": "0x26b9bfb01cad0a36c62e3c8aae064a50e510bbbde6b62b0be4b8f6dc6377dad4"
                        },
                        "nonce": 5,
                        "status": "REJECTED",
                        "rejectionReason": "BALANCE_TOO_LOW",
                        "operationHash": "0x11ce804a93a1904549434374d05a91452e6fd64d0bcef1d2fc0cc0d56df94072"
                    }
                ]
            }
        }
        ```

## pazkp_listMoneyOrderRedemptions

List money orders submitted for redemption.

### Parameters

!!! important

    Use `null` if you don't want to specify a value for an optional parameter.

* `nonce`: *integer* - transaction nonce

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - (optional) 32-bit account ID of the sender

* `toAccountId`: *integer* - (optional) 32-bit account ID of the receiver

* `status`: *string* - (optional) status of the money order

* `minAmount`: *integer* - (optional) filter results by the minimum transaction value

* `maxAmount`: *integer* - (optional) filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if there are more results available, which haven't been displayed.

* `items`: *array of objects* - money orders submitted for redemption:

    * `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
      `blindingFactor`

    * `nonce`: *integer* - transaction nonce

    * `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
      money order batch, and 2 bytes are for the index of the money order in the batch.

    * `operationHash`: *string* - hash of the operation

    * `status`: *string* - status of the redemption, for example `EXECUTED` or `PENDING`

    * `batchHeight`: *integer* - the unique batch index in the rollup that contains the transaction, following ascending ordinal numbering

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_listMoneyOrderRedemptions","params":[3,2,null,1,null,null,null],"id":133}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 133,
            "result": {
                "hasMoreResults": false,
                "items": [
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 0,
                            "amount": 200,
                            "blindingFactor": "0x03df039c5641ed40cd9eddc5e2a81e3fa18b38e32ddfa5912cb57d7b94cbfdb6"
                        },
                        "nonce": 3,
                        "moneyOrderTotalIndex": 131073,
                        "operationHash": "0x18ce420855e7c899bcd6a86474d3b4c2917bf1ac1f3dd17be54938c5587a8e82",
                        "status": "EXECUTED",
                        "batchHeight": 6
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 1,
                            "amount": 50,
                            "blindingFactor": "0x193d2c8d8fd20908f844a4d8b18cd07b4450710c49f6d2e12a62542625ff95e3"
                        },
                        "nonce": 4,
                        "moneyOrderTotalIndex": 196608,
                        "operationHash": "0x0f745f7245fd4140ad718e9ea8700173571e5d5ea5fdaf949a9abe313c09dd12",
                        "status": "REJECTED",
                        "rejectionReason": "REPLACED_BY_OPERATION_WITH_SAME_NONCE"
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 1,
                            "amount": 50,
                            "blindingFactor": "0x193d2c8d8fd20908f844a4d8b18cd07b4450710c49f6d2e12a62542625ff95e3"
                        },
                        "nonce": 4,
                        "moneyOrderTotalIndex": 196608,
                        "operationHash": "0x2216dc18f5f303babbaffa809ed209984091cde2bf6b0e6bf727f3716261f7e5",
                        "status": "EXECUTED",
                        "batchHeight": 7
                    }
                ]
            }
        }
        ```

## pazkp_listMoneyOrdersReceived

List money orders available to redeem that are already finalized on the blockchain.

### Parameters

!!! important

    Use `null` if you don't want to specify a value for an optional parameter.

* `fromMoneyOrderTotalIndex`: *integer* - starting index for the money order batch

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - (optional) 32-bit account ID of the sender

* `toAccountId`: *integer* - 32-bit account ID of the receiver

* `status`: *string* - (optional) status of the money order. Valid options are `AVAILABLE_TO_REDEEM` and
    `REDEEMED`

* `minAmount`: *integer* - (optional) filter results by the minimum transaction value

* `maxAmount`: *integer* - (optional) filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if there are more results available, which haven't been displayed.

* `items`: *array of objects* - money orders available to redeem:

    * `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
        `blindingFactor`

    * `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
        money order batch, and 2 bytes are for the index of the money order in the batch.

    * `batchHeight`: *integer* - the unique batch index in the rollup that contains the transaction, following ascending ordinal numbering

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_listMoneyOrdersReceived","params":[0,10,null,1,null,null,null],"id":111}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 111,
            "result": {
                "hasMoreResults": false,
                "items": [
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 0,
                            "amount": 100,
                            "blindingFactor": "0x22879dd7ce3e297b879a52225026f256d8758bb700c40d53b769472218cfe740"
                        },
                        "moneyOrderTotalIndex": 65536,
                        "batchHeight": 2
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 1,
                            "amount": 50,
                            "blindingFactor": "0x204f2eff2a733beb0bf490ee3e60ad108cef8f87e7d2a9a5e0f36dc8888eb295"
                        },
                        "moneyOrderTotalIndex": 131072,
                        "batchHeight": 3
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 0,
                            "amount": 200,
                            "blindingFactor": "0x03df039c5641ed40cd9eddc5e2a81e3fa18b38e32ddfa5912cb57d7b94cbfdb6"
                        },
                        "moneyOrderTotalIndex": 131073,
                        "batchHeight": 3
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 0,
                            "toAccountId": 1,
                            "tokenId": 1,
                            "amount": 50,
                            "blindingFactor": "0x193d2c8d8fd20908f844a4d8b18cd07b4450710c49f6d2e12a62542625ff95e3"
                        },
                        "moneyOrderTotalIndex": 196608,
                        "batchHeight": 4
                    }
                ]
            }
        }
        ```

## pazkp_listOutboundTransferCreations

List all outbound transfers for which you are the sender.

### Parameters

!!! important

    Use `null` if you don't want to specify a value for an optional parameter.

* `nonce`: *integer* - transaction nonce

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - 32-bit account ID of the sender

* `toEthereumAddress`: *integer* - (optional) 32-bit account ID of the receiver

* `status`: *string* - (optional) status of the outbound transfer

* `minAmount`: *integer* - (optional) filter results by the minimum transaction value

* `maxAmount`: *integer* - (optional) filter results by the maximum transaction value

### Returns

`result`: *object* - outbound transfer details:

* `hasMoreResults`: *boolean* - `true` if there are more results available, which haven't been displayed

* `items`: *array of objects* - outbound transfer records:

    * `fromAccountId`: *integer* - 32-bit account ID of the sender

    * `toEthereumAddress`: *integer* - 20-byte Ethereum address of the receiver

    * `tokenId`: *integer* - token type ID

    * `amount`: *integer* - token amount for the transaction

    * `nonce`: *integer* - transaction nonce

    * `operationHash`: *string* - hash of the operation

    * `status`: *string* - status of the outbound transfer, for example `PENDING` or `EXECUTED`

    * `rejectionReason`: *string* - rejection reason details, when status of the outbound transfer is `REJECTED`

    * `batchHeight`: *integer* - the unique batch index in the rollup that contains the transaction, following ascending ordinal numbering

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_listOutboundTransferCreations","params":[0,1,1,null,null,null,null],"id":102}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 102,
            "result": {
                "hasMoreResults": true,
                "items": [
                    {
                        "fromAccountId": 1,
                        "toEthereumAddress": "0x0f38616f3f7ca7316aceea7adb3fd53fa2527026",
                        "tokenId": 1,
                        "amount": 5,
                        "nonce": 5,
                        "operationHash": "0x1780f941d14acb15d4c906c6b1b54ba77d9d4d6558079183b25f8731424ee498",
                        "status": "EXECUTED",
                        "batchHeight": 8
                    }
                ]
            }
        }
        ```

## pazkp_redeemMoneyOrder

Open a money order and claim the funds locked within.

### Parameters

* `fromAccountId`: *integer* - 32-bit account ID of the sender

* `toAccountId`: *integer* - 32-bit account ID of the receiver

* `tokenId`: *integer* - token type ID

* `amount`: *integer* - token amount for the transaction

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation.

* `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
    money order batch, and 2 bytes are for the index of the money order in the batch

* `nonce`: *integer* - transaction nonce

* `encryptedMoneyOrderRedeem`: *string* - money order redemption encrypted, using the receiver's `jointEncryptionKey`

* `randomizationFactor`: *string* - value produced during encryption used to check whether data has been
    encrypted correctly

* `signature`: *string* - signature of sender's account

* `merkleProof`: *string* - Merkle Proof connecting the money order hash to the batch root hash

### Returns

`result`: *object* - money order redemption details

* `operationHash`: *string* - hash of the operation

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"pazkp_redeemMoneyOrder","params":[0,1,0,100,"0x22879dd7ce3e297b879a52225026f256d8758bb700c40d53b769472218cfe740",65536,1,"0x0000000000000003209096e27e1265ebc551c57833fddf8486cc57e24b76e982b071d07a8b2acec0078f69d2562aecf40d2e364607e14aa8b71bb642605d31b1bd396c9ca054e7df1d0a7c98d9976930a77a66e1678cefacac54e625122dbeb7c662ae366df31eab136af2e3db6fa5981ea564d6ea39a385a3a13e970720d1424e4fe358bca18af61d0a7c98d9976930a77a66e1678cefacac54e625122dbeb7c662ae366df31eab136af2e3db6fa5981ea564d6ea39a385a3a13e970720d1424e4fe358bca18af6000000000000000110683825db0f0dc939f4ecf23f57779ef81ab61e7186765c742a53c996dd1710","0x28787ac1792dd7e92089fa7f64e6f852785008284986b93a6257d0e3725f8744","0x1b20db3c09b5e26fd1d8e9b2061f02d6d3ca9b5f9cfd12642e774b3f2bafa9322b16fb351a58b63f0c8e011a206bb6cce49e8671a5d92c76206bbe2ba3a14f08035848e2723f5d4dfe30fbdd53346ab93bb6f843aa51dc803cde4131313c4f08",null],"id":61}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": 61,
            "result": {
                "operationHash": "0x105cbc9a6c55a5a525c81993a1a3f6c0d0b9786a38a76b63f5e6652423c946f2"
            }
        }
        ```
