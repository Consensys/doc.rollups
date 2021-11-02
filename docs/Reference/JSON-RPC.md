---
Description: JSON-RPC API methods
---

# ConsenSys Rollups API methods

## `createAccount`

Creates an account on the rollup.

!!! tip

    You need to wait for the account to finalized on the blockchain before creating money orders.

    Use [`findAccountId`](#findaccountid) to obtain the account ID and determine whether the account is ready to use.

### Parameters

* `publicKey`: *string* - 64-byte account public key

* `jointEncryptionKey`: *string* - 128-bytes encryption key based on the user's public key and
    operator's public key assembled using the ConsenSys Rollups cryptographic library.

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation.

* `encryptedBlindingFactor`: *string* - blinding factor encrypted using `jointEncryptionKey`.

* `randomizationFactor`: *string* - value produced during encryption used to check whether data has been
    encrypted correctly.

* `signature`: *string* - 96-byte account signature.

### Returns

`result`: *string* - status of the account creation, for example `REGISTERED`; otherwise an error, for example
`PUBLIC_KEY_ALREADY_EXISTS`.

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"createAccount","params":["0xd30fcb74638294dce12e113d4aeeba4a4270e3519b85cb2890e6904bde54a72a8289d97215cd827788cde39736ddd9980909d90dbba1a0f7dde5aa758d98951e","0x19b8a46446480f5ec2234bb7a3d50eeed10c002d046bf9000984985bd6d883d86adfe49ffff8756c00ecda4dcbed4838281072b621e1ef6f1770404e9bb8f21170e2b71343da962a1c3970e9fd365304d2a293dbe2aabc61e1c39d3e1f3109cd482991de12b892d91cf79d77b8635541bfbbeb32b20fbd8f8d339d545eb74f76d7145cfce061cf1c66c26385141968bae792f96aaf9d1b5ae645087127c8c3fc43d35029172581ab4610c648392dc136c03d325b2ccc6624d984a7bd9d8ef99d","177c429fd0b4b976fe46874ce33308e879f395463dfb88ae70795ed8dada5bab","0xa4ee6122243282f77028d588eb6684e503d35ae5f6f4ae0ffad2d571c767041ebb028bfd0f3bd7504666b872afc0b8e8739cbb5c6c825d27399abe1ce672e64bf370bdac8e807b36cb4edf70aa8efb1fa2a09729b4cb4fcd324f8c1088a1459e"],"id":0}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": "0",
            "result": {
                "status": "REGISTERED"
            }
        }
        ```

## createMoneyOrder

Create a money order to transfer an amount between two accounts. Both accounts must be in the
rollup.

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

`result`: *object* - money order hash details:

* `moneyOrderHash`: *string* - money order hash
* `moneyOrderCreationHash`: *string* - hash containing the money order and nonce

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"createMoneyOrder","params":[32,33,0,10,"0x1aa22d3baa52c393a40428df86a9a4c0b35963db9cc48f31427e962ec9f132f9",1,"0x2bbf79bf681f25fdde712025f4372ddde73727b7d047681054d1c92e5c3424802ee7cffc8212e2dd546184f467f85d5d5b627e6e3c413186b554755fce65c8c6056d635517b6d63c43abf11c67eab476bbf3455a66d64039b26a85e3a0751f1e"],"id":9}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": "9",
            "result": {
                "moneyOrderHash": "0x301f82f185174801f1d1db9bb3fc971d9039b587d5af40573762bdd5463b40d3",
                "moneyOrderCreationHash": "0x1a58607ba07124a2e6a431a3dd4ed5c92f9a5c71f0c09a61ca8e3c9c125c58a6"
            }
        }
        ```

## findAccountId

Get the ID assigned to an account. You need to determine the ID of an account to create a money
order.

### Parameters

* `publicKey`: *string* - 64-byte account public key

### Returns

`result`: *object* - account details with the following fields:

* `status`: *string* - account status. Status types are `PENDING`, `ACTIVE`, or an error (for example: `PUBLIC_KEY_NOT_FOUND`).
* `id`: *integer* - account ID.

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"findAccountId","params":["0xd30fcb74638294dce12e113d4aeeba4a4270e3519b85cb2890e6904bde54a72a8289d97215cd827788cde39736ddd9980909d90dbba1a0f7dde5aa758d98951e"],"id":1}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": "1",
            "result": {
                "status": "ACTIVE",
                "id": 32
            }
        }
        ```

## getAccountState

Returns the latest finalized account state on the blockchain.

### Parameters

* `accountId`: *integer* - account ID

### Returns

`result`: *object* - account state details:

* `id`: *string* - account ID

* `publicKey`: *string* - account public key

* `jointEncryptionKey`: *string* - 128-bytes encryption key based on the user's public key and
    operator's public key assembled using the ConsenSys Rollups cryptographic library

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation

* `nonce`: *integer* - nonce of the most recent account operation

* `redemptionIndex`: *string* - redemption index of the most recent redeemed money order

* `status`: *string* - status of the account. Status types are PENDING, ACTIVE, or an error.

* `balances`: *object* - a map of the token balance for each token index.

* `rootHash`: *string* - root hash of the account in the merkle tree

* `merkleProof`: *array* - merkle proof of the account in the merkle tree

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"getAccountState","params":[32],"id":7}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": "7",
            "result": {
                "id": 32,
                "publicKey": "0xd30fcb74638294dce12e113d4aeeba4a4270e3519b85cb2890e6904bde54a72a8289d97215cd827788cde39736ddd9980909d90dbba1a0f7dde5aa758d98951e",
                "jointEncryptionKey": "0x19b8a46446480f5ec2234bb7a3d50eeed10c002d046bf9000984985bd6d883d86adfe49ffff8756c00ecda4dcbed4838281072b621e1ef6f1770404e9bb8f21170e2b71343da962a1c3970e9fd365304d2a293dbe2aabc61e1c39d3e1f3109cd482991de12b892d91cf79d77b8635541bfbbeb32b20fbd8f8d339d545eb74f76d7145cfce061cf1c66c26385141968bae792f96aaf9d1b5ae645087127c8c3fc43d35029172581ab4610c648392dc136c03d325b2ccc6624d984a7bd9d8ef99d",
                "blindingFactor": "0x177c429fd0b4b976fe46874ce33308e879f395463dfb88ae70795ed8dada5bab",
                "nonce": 0,
                "redemptionIndex": 0,
                "status": "ACTIVE",
                "balances": {
                    "0": 1000000000,
                    "1": 1000000000
                },
                "rootHash": "0x1fd9937cf93b2eac62aee9dd726e38e5d5f2d38d198cba62cfad1ee268f6e869",
                "merkleProof": [
                    "0x23a6f0ada12c09b5e9ee941d484a54b37aeb512bf22a37ce1d742d98824df936",
                    "0x0b0fdabbfef8537a30947af39592d3e6586899fc3a4c553ebc6c225bfde5b291",
                    "0x167fad9994a4b04bfadf4722c7cb71c4b932fa00afe9bd111cf8be399612affb",
                    "0x037f79e9f450c48330d9e94ea21d0fa121de36a66ff7d74ba796249b6ef388da",
                    "0x017baba79690a9b670bc48cac82822ac56ad6d3f4023d1c507e6c8acd5dff5cd",
                    "0x2db1152a292d1ed507c90c2ffc8242a9023c4f07349fa2a2b3c1076237cd3d44",
                    "0x2f6e97732fe1f5d5a381f81826c0e69f57f89f939f0ebfd8ca490d5511101233",
                    "0x2d5cbd057a50567c661399f4df5c4cd1593dd281e31f03f6fb378d4740facd7c",
                    "0x03bb46cba477195f542e50a2ce2855856a063d8f2ff31983ef0ec9827096b0ac",
                    "0x014d8bfb32d8b53f32ed86c44d58bb2a2ca8eeb28708160a538bee9f020cf071",
                    "0x021190117d000355b458b4bb41a2cbfb3773851ad51d60bf6e6ee2dede23a278",
                    "0x2a6cf7f8602039f58fbf322f856d63dce9bdc399cc80101588be5886bd91aebd",
                    "0x24e4d8f1e2c26b2a762a9553f59307aec18b5cb9feff9d9fe5c3081e76a18db0",
                    "0x15275d1fb2a46c02929bc28003a490fd1200fa10ebf2b53d5e81b32f074ce081",
                    "0x0b033b225e93b38c1db4bd81b76541745532b5cc367b55e605999240180b0727",
                    "0x060016c05c2bb08e9fe3d0d11f8003f0e93c1fb0c56f9b0847128fb3a2e4e615",
                    "0x096d391b44d4d10b4bceaa3fbabcacae5b6609d2619e8cedea7b36eac5d12c85",
                    "0x0ad07be08a9c1103e9667427d1d00cd70f08244d655156914151f51172833b6c",
                    "0x200ba26f0e63f8b9aac5aa90edf8de81d32c08a9647ad335f904cd1c7d5d05e7",
                    "0x2e2aa88e3036f8f80bdc279779e7e0165204fcf69da8f885e40ee14ba3ab558e",
                    "0x02075b0c5a7fbc184f9b876bf2193769121e7c2eac808da352372757140a8bcd",
                    "0x0441daca0462db9fc7bdcc799e6a3cd2b4454a3449eba3bab6bf6191dbc9e196",
                    "0x113bcde42f6357c971a6c6cb9de1222f27bf02614ac6b5a25a661145f233c54c",
                    "0x1a775b6db17fcc95be1b0ad6acf99924aa084b3be755a2c35b4a7059e40ff963",
                    "0x1b417693fd51f7b0a0b960e58edc35e3c43a0fe06d14e265f1d11721654ae74e",
                    "0x285b206237bf522255413c8e911424afa35a41cc2a254ccfd8ae73ff8d21a377",
                    "0x2887db3d28125e0bbb1b22f42c9f19eb3df3533dd3c16b55b5042cc604dd9f48",
                    "0x1b64b843d1f61e1bf58b2db4fd9dcfef59f95b4f15db93b81a7fe295381d6809",
                    "0x005cdf59e85a8504be65bf52fa0988d1ce63d46c60b2e1051c3d85989369912a"
                ]
            }
        }
        ```

## listMoneyOrderCreations

List all money orders for which you are the issuer.

### Parameters

!!! important

    Use `null` if you don't want to specify a value for an optional parameter.

* `nonce`: *integer* - nonce of the account operation

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - 32-bit account ID of the sender

* `toAccountId`: *integer* - (optional) 32-bit account ID of the receiver

* `status`: *string* - (optional) status of the money order

* `minAmount`: *integer* - (optional) filter results by the minimum transaction value

* `maxAmount`: *integer* - (optional) filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if more results are available which hasn't been displayed.

* `items`: *array of objects* - money order creation records:

    * `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
        `blindingFactor`
    * `nonce`: *integer* - transaction nonce
    * `status`: *string* - status of the money order, for example `PENDING` or `EXECUTED`

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"listMoneyOrderCreations","params":[1,10,32,null,null,null,null],"id":10}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": "10",
            "result": {
                "hasMoreResults": false,
                "items": [
                    {
                        "moneyOrder": {
                            "fromAccountId": 32,
                            "toAccountId": 33,
                            "tokenId": 0,
                            "amount": 10,
                            "blindingFactor": "0x1aa22d3baa52c393a40428df86a9a4c0b35963db9cc48f31427e962ec9f132f9"
                        },
                        "nonce": 1,
                        "status": "PENDING"
                    }
                ]
            }
        }
        ```

## listMoneyOrdersReceived

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
.
* `minAmount`: *integer* - (optional) filter results by the minimum transaction value

* `maxAmount`: *integer* - (optional) filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if more results are available which hasn't been displayed.

* `items`: *array of objects* - money orders available to redeem:

    * `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
        `blindingFactor`
    * `moneyOrderHash`: *string* - money order hash
    * `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
        money order batch, and 2 bytes are for the index of the money order in the batch.
    * `merkleProof`: *array* - merkle proof of the account in the merkle tree
    * `batchStateRootHash`: *string* - rollup `rootHash` that corresponds to the batch in which the money order
        was included

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"listMoneyOrdersReceived","params":[0,10,null,33,null,null,null],"id":51}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": "51",
            "result": {
                "hasMoreResults": false,
                "items": [
                    {
                        "moneyOrder": {
                            "fromAccountId": 32,
                            "toAccountId": 33,
                            "tokenId": 0,
                            "amount": 10,
                            "blindingFactor": "0x1b2f1687993e5b8150df67553db828dae0c54fe8fc331cd820fef7b19d43e45a"
                        },
                        "moneyOrderHash": "0x1f09a9d97feaed58a4acb3a5fc5ef8ee5089d0076936b930223fff9c5c438ee9",
                        "moneyOrderTotalIndex": 115964116992,
                        "merkleProof": [
                            "0x0000000000000000000000000000000000000000000000000000000000000000",
                            "0x2d47261abcaff616dc7ea5563d9fd9d428f0c6ce6cdfd4a972fe69bcf15b921c",
                            "0x0a32ca815e4f1e29cd8f25f29cdbf6c5356ec2e34afde86732f0ff4f2ead940d",
                            "0x2dc4da5b780d7ed6013783e24d207c21cd36cf888791f75010f7723ce312c9e1",
                            "0x13ad7f2768c62fd7f77d6106d01e722cfa05b5b86da4b15c3df0b2cfd636b7c7",
                            "0x2fb2f8b9cf3db951d873106bbcc5a7cdb2fb904a81d5a9931e94a77acc88919e",
                            "0x2d12f4a1c13f14acefc55368f101c8bf8c0087277ddfd962b690d7189912b55a",
                            "0x1ea6dc228eb60f4847c35943252e80b513c0d3cb07cf1a6c520b3718eebbbb3c",
                            "0x08c1488667c6b002fc352ed01b3b572354604e3f9b877634ce1d6d5f8dba3016",
                            "0x1969276cbf8d8affeb36547045ce6db660b8ac2b6b970c85e333324d3e551ca9",
                            "0x1ca8d15ddb518d6218deb8769407a39af349b94760db12392fc127592a1ea3da",
                            "0x2ba0a8ee2ffd028add1d1cb8288869e058d04f44729710e742b28431f43a4f06",
                            "0x1c620c4bf1748c16111666b4fd0ca2d35374780fa0c9b12c56f4fe41f0f0c16f",
                            "0x2f90d0d16fd308f07a28228b345017fa85b4072f07f3d6106f2e48e4e7dfe286",
                            "0x231b35c02d455e0f869c55de2599e6158828c86aded71ce7b52fc2247e2f25fb",
                            "0x2cc604d4d64be07fcfc2bd4f42fa7a84065c9d0380e54972431509f5e100034a"
                        ],
                        "batchStateRootHash": "0x129b029aea8c4d963602abe485ea3b02afe481160ef83c133d4088ced55c3557"
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 32,
                            "toAccountId": 33,
                            "tokenId": 0,
                            "amount": 10,
                            "blindingFactor": "0x29c37ede5648785304a509a9e99d0f052e5eb088c8da511d447b3303cae55210"
                        },
                        "moneyOrderHash": "0x11e8edb482a93d5f2c9ca77d5e79c74d0039d820e60fee9f307d8d2ffc741989",
                        "moneyOrderTotalIndex": 120259084288,
                        "merkleProof": [
                            "0x0000000000000000000000000000000000000000000000000000000000000000",
                            "0x2d47261abcaff616dc7ea5563d9fd9d428f0c6ce6cdfd4a972fe69bcf15b921c",
                            "0x0a32ca815e4f1e29cd8f25f29cdbf6c5356ec2e34afde86732f0ff4f2ead940d",
                            "0x2dc4da5b780d7ed6013783e24d207c21cd36cf888791f75010f7723ce312c9e1",
                            "0x13ad7f2768c62fd7f77d6106d01e722cfa05b5b86da4b15c3df0b2cfd636b7c7",
                            "0x2fb2f8b9cf3db951d873106bbcc5a7cdb2fb904a81d5a9931e94a77acc88919e",
                            "0x2d12f4a1c13f14acefc55368f101c8bf8c0087277ddfd962b690d7189912b55a",
                            "0x1ea6dc228eb60f4847c35943252e80b513c0d3cb07cf1a6c520b3718eebbbb3c",
                            "0x08c1488667c6b002fc352ed01b3b572354604e3f9b877634ce1d6d5f8dba3016",
                            "0x1969276cbf8d8affeb36547045ce6db660b8ac2b6b970c85e333324d3e551ca9",
                            "0x1ca8d15ddb518d6218deb8769407a39af349b94760db12392fc127592a1ea3da",
                            "0x2ba0a8ee2ffd028add1d1cb8288869e058d04f44729710e742b28431f43a4f06",
                            "0x1c620c4bf1748c16111666b4fd0ca2d35374780fa0c9b12c56f4fe41f0f0c16f",
                            "0x2f90d0d16fd308f07a28228b345017fa85b4072f07f3d6106f2e48e4e7dfe286",
                            "0x231b35c02d455e0f869c55de2599e6158828c86aded71ce7b52fc2247e2f25fb",
                            "0x2cc604d4d64be07fcfc2bd4f42fa7a84065c9d0380e54972431509f5e100034a"
                        ],
                        "batchStateRootHash": "0x0035396587afb5659b5a388891399bd61f0e7c77c419a1ee3a4a04d0c583aebc"
                    }
                ]
            }
        }
        ```

## listMoneyOrderRedemptions

List money orders submitted for redemption.

### Parameters

!!! important

    Use `null` if you don't want to specify a value for an optional parameter.

* `nonce`: *integer* - nonce of the account operation

* `softLimit`: *integer* - maximum number of results to return

* `fromAccountId`: *integer* - (optional) 32-bit account ID of the sender

* `toAccountId`: *integer* - (optional) 32-bit account ID of the receiver

* `status`: *string* - (optional) status of the money order

* `minAmount`: *integer* - (optional) filter results by the minimum transaction value

* `maxAmount`: *integer* - (optional) filter results by the maximum transaction value

### Returns

`result`: *object* - money order details:

* `hasMoreResults`: *boolean* - `true` if more results are available which hasn't been displayed.

* `items`: *array of objects* - money orders submitted for redemption:

    * `moneyOrder`: *object* - object containing the `fromAccountId`, `toAccountId`, `tokenId`, `amount`, and
        `blindingFactor`
    * `nonce`: *integer* - transaction nonce
    * `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
        money order batch, and 2 bytes are for the index of the money order in the batch.
    * `merkleProof`: *array* - merkle proof of the account in the merkle tree
    * `status`: *string* - status of the redemption, for example `EXECUTED` or `PENDING`
    * `batchStateRootHash`: *string* -

!!! example

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"listMoneyOrderRedemptions","params":[0,10,null,33,null,null,null],"id":93}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": "93",
            "result": {
                "hasMoreResults": false,
                "items": [
                    {
                        "moneyOrder": {
                            "fromAccountId": 32,
                            "toAccountId": 33,
                            "tokenId": 0,
                            "amount": 10,
                            "blindingFactor": "0x158e2020660b63282d1c583a4a1f938098b22e4311a2f4f0ab816221635118b0"
                        },
                        "nonce": 1,
                        "moneyOrderTotalIndex": 133143986176,
                        "merkleProof": [
                            "0x0000000000000000000000000000000000000000000000000000000000000000",
                            "0x2d47261abcaff616dc7ea5563d9fd9d428f0c6ce6cdfd4a972fe69bcf15b921c",
                            "0x0a32ca815e4f1e29cd8f25f29cdbf6c5356ec2e34afde86732f0ff4f2ead940d",
                            "0x2dc4da5b780d7ed6013783e24d207c21cd36cf888791f75010f7723ce312c9e1",
                            "0x13ad7f2768c62fd7f77d6106d01e722cfa05b5b86da4b15c3df0b2cfd636b7c7",
                            "0x2fb2f8b9cf3db951d873106bbcc5a7cdb2fb904a81d5a9931e94a77acc88919e",
                            "0x2d12f4a1c13f14acefc55368f101c8bf8c0087277ddfd962b690d7189912b55a",
                            "0x1ea6dc228eb60f4847c35943252e80b513c0d3cb07cf1a6c520b3718eebbbb3c",
                            "0x08c1488667c6b002fc352ed01b3b572354604e3f9b877634ce1d6d5f8dba3016",
                            "0x1969276cbf8d8affeb36547045ce6db660b8ac2b6b970c85e333324d3e551ca9",
                            "0x1ca8d15ddb518d6218deb8769407a39af349b94760db12392fc127592a1ea3da",
                            "0x2ba0a8ee2ffd028add1d1cb8288869e058d04f44729710e742b28431f43a4f06",
                            "0x1c620c4bf1748c16111666b4fd0ca2d35374780fa0c9b12c56f4fe41f0f0c16f",
                            "0x2f90d0d16fd308f07a28228b345017fa85b4072f07f3d6106f2e48e4e7dfe286",
                            "0x231b35c02d455e0f869c55de2599e6158828c86aded71ce7b52fc2247e2f25fb",
                            "0x2cc604d4d64be07fcfc2bd4f42fa7a84065c9d0380e54972431509f5e100034a"
                        ],
                        "status": "EXECUTED",
                        "batchStateRootHash": "0x291c3279cfa0d35060c0ec9b4dd12e5b8234d48b2c35b7536b2f0726c06a43e7"
                    },
                    {
                        "moneyOrder": {
                            "fromAccountId": 32,
                            "toAccountId": 33,
                            "tokenId": 0,
                            "amount": 10,
                            "blindingFactor": "0x1114cf639501ba14fe9db7e6ffa23bbb7be0c8ba726e5b1cc807b20b6150629f"
                        },
                        "nonce": 2,
                        "moneyOrderTotalIndex": 137438953472,
                        "merkleProof": [
                            "0x0000000000000000000000000000000000000000000000000000000000000000",
                            "0x2d47261abcaff616dc7ea5563d9fd9d428f0c6ce6cdfd4a972fe69bcf15b921c",
                            "0x0a32ca815e4f1e29cd8f25f29cdbf6c5356ec2e34afde86732f0ff4f2ead940d",
                            "0x2dc4da5b780d7ed6013783e24d207c21cd36cf888791f75010f7723ce312c9e1",
                            "0x13ad7f2768c62fd7f77d6106d01e722cfa05b5b86da4b15c3df0b2cfd636b7c7",
                            "0x2fb2f8b9cf3db951d873106bbcc5a7cdb2fb904a81d5a9931e94a77acc88919e",
                            "0x2d12f4a1c13f14acefc55368f101c8bf8c0087277ddfd962b690d7189912b55a",
                            "0x1ea6dc228eb60f4847c35943252e80b513c0d3cb07cf1a6c520b3718eebbbb3c",
                            "0x08c1488667c6b002fc352ed01b3b572354604e3f9b877634ce1d6d5f8dba3016",
                            "0x1969276cbf8d8affeb36547045ce6db660b8ac2b6b970c85e333324d3e551ca9",
                            "0x1ca8d15ddb518d6218deb8769407a39af349b94760db12392fc127592a1ea3da",
                            "0x2ba0a8ee2ffd028add1d1cb8288869e058d04f44729710e742b28431f43a4f06",
                            "0x1c620c4bf1748c16111666b4fd0ca2d35374780fa0c9b12c56f4fe41f0f0c16f",
                            "0x2f90d0d16fd308f07a28228b345017fa85b4072f07f3d6106f2e48e4e7dfe286",
                            "0x231b35c02d455e0f869c55de2599e6158828c86aded71ce7b52fc2247e2f25fb",
                            "0x2cc604d4d64be07fcfc2bd4f42fa7a84065c9d0380e54972431509f5e100034a"
                        ],
                        "status": "EXECUTED",
                        "batchStateRootHash": "0x1873ad45d7d0c84b662333b8b1c95ab8ca91f98104c1ebda4b0e4410ce3c6ffc"
                    }
                ]
            }
        }
        ```

## redeemMoneyOrder

Open a money order and claim the funds locked within.

### Parameters

* `fromAccountId`: *integer* - 32-bit account ID of the sender.

* `toAccountId`: *integer* - 32-bit account ID of the receiver.

* `tokenId`: *integer* - token type ID.

* `amount`: *integer* - token amount for the transaction.

* `blindingFactor`: *string* - 32-byte blinding factor for obfuscation.

* `moneyOrderTotalIndex`: *integer* - 6 byte integer where 4 bytes contain the money order batch ID of the
    money order batch, and 2 bytes are for the index of the money order in the batch.

* `nonce`: *integer* - nonce of the most recent account operation.

* `encryptedMoneyOrderRedeem`: *string* - money order redemption encrypted, using the receiver's `jointEncryptionKey`.

* `randomizationFactor`: *string* - value produced during encryption used to check whether data has been
    encrypted correctly.

* `signature`: *string* - signature of sender's account.

* `merkleProof`: *string* - Merkle Proof connecting the money order hash to the batch root hash.

### Returns

`result`: *object* - money order hash details:

* `moneyOrderHash`: *string* - money order hash
* `moneyOrderRedemptionHash`: *string* - hash of the money order hash and nonce

    === "curl HTTP request"

        ```bash
        curl -X POST --data '{"jsonrpc":"2.0","method":"redeemMoneyOrder","params":[32,33,0,10,"0x053eb868b76733fc0fdcab4aaa1d857f02709775e0481ba014221cb9a78ae337",128849018880,2,"0x2fb8667c1a8b64228da4f6c3bb6c2e222b4975151c724a9e3e559598067a346f2fe261638d627e55848704b88e831adb9b8f366322d14d1e82f69f0764707ce20333fadcad3dd56c2bfa4107984ef2ea2e2fd5c595f19dcb58113a0925d3aca4",["0x0000000000000000000000000000000000000000000000000000000000000000","0x2d47261abcaff616dc7ea5563d9fd9d428f0c6ce6cdfd4a972fe69bcf15b921c","0x0a32ca815e4f1e29cd8f25f29cdbf6c5356ec2e34afde86732f0ff4f2ead940d","0x2dc4da5b780d7ed6013783e24d207c21cd36cf888791f75010f7723ce312c9e1","0x13ad7f2768c62fd7f77d6106d01e722cfa05b5b86da4b15c3df0b2cfd636b7c7","0x2fb2f8b9cf3db951d873106bbcc5a7cdb2fb904a81d5a9931e94a77acc88919e","0x2d12f4a1c13f14acefc55368f101c8bf8c0087277ddfd962b690d7189912b55a","0x1ea6dc228eb60f4847c35943252e80b513c0d3cb07cf1a6c520b3718eebbbb3c","0x08c1488667c6b002fc352ed01b3b572354604e3f9b877634ce1d6d5f8dba3016","0x1969276cbf8d8affeb36547045ce6db660b8ac2b6b970c85e333324d3e551ca9","0x1ca8d15ddb518d6218deb8769407a39af349b94760db12392fc127592a1ea3da","0x2ba0a8ee2ffd028add1d1cb8288869e058d04f44729710e742b28431f43a4f06","0x1c620c4bf1748c16111666b4fd0ca2d35374780fa0c9b12c56f4fe41f0f0c16f","0x2f90d0d16fd308f07a28228b345017fa85b4072f07f3d6106f2e48e4e7dfe286","0x231b35c02d455e0f869c55de2599e6158828c86aded71ce7b52fc2247e2f25fb","0x2cc604d4d64be07fcfc2bd4f42fa7a84065c9d0380e54972431509f5e100034a"]],"id":67}' <IP>:<PORT>
        ```

    === "JSON result"

        ```json
        {
            "jsonrpc": "2.0",
            "id": "67",
            "result": {
                "moneyOrderHash": "0x165827bc9aadfe5643fecad5177ea604408e6afbcc3addee455013415fbb830e",
                "moneyOrderRedemptionHash": "0x053837ed723028ecfda06712916326d89d1e996d73c85800aa1de612453d6005"
            }
        }
        ```
