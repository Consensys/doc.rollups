---
Description: Configure key management
---

# Key management

Configure ConsenSys Rollups operator keys to be managed in your local filesystem or by
[Quorum Key Manager (QKM)](https://docs.quorum-key-manager.consensys.net/en/stable/).

You can do this by updating the [engine configuration file](Configuration-File.md#engine-configuration-file).

## Configure filesystem

To configure the filesystem to manage Rollups operator keys, create a file containing the operator's private and
encryption keys as in the following example:

!!! example "`operator_1.acc`"

    ```JSON
    {
        "account_key": {
            "priv_key": "202454d1b4e72c41ebf58150030f649648d3cf5590297fb6718e27039ed9c86d"
        },
        "encryption_key": {
            "key": "202454d1b4e72c41ebf58150030f649648d3cf5590297fb6718e27039ed9c86d"
        }
    }
    ```


Then, set the
[key management configuration options](../../Reference/Configuration-File.md#key_managementaccount_key) in the engine
configuration file as in the following example.

!!! example "Filesystem key management configuration"

    ```toml
    [key_management.account_key]
        manager_type = "Filesystem"

        # Path to the file containing the operator's private key
        operator_key_path = "node-data/test/keys/operator_1.acc"

    [key_management.encryption_key]
        manager_type = "Filesystem"

        # Path to the file containing the operator's encryption key
        encryption_key_path = "node-data/test/keys/operator_1.acc"
    ```

## Configure Quorum Key Manager

To configure QKM to manage Rollups operator keys, first
[configure QKM key stores](https://docs.quorum-key-manager.consensys.net/en/stable/HowTo/Use-Manifest-File/Store/) to
store the operator's private and encryption keys.

Next, create a file containing the operator's Ethereum address and the ID of the operator's encryption key as in the
following example:

!!! example "`operator_1.acc`"

    ```JSON
    {
        "account_key": {
            "addr": "0xd0584d4d37157f7105a4b41ed8ecbdfafdb2547f"
        },
        "encryption_key": {
            "key_id": "operator_1_enc_key"
        }
    }
    ```


Then, set the
[key management configuration options](../../Reference/Configuration-File.md#key_managementaccount_key) in the engine
configuration file as in the following example.

!!! example "QKM configuration"

    ```toml
    [key_management.account_key]
        manager_type = "Qkm"

        # URL to QKM service
        qkm_url = "http://qkm:8080"

        # Name of the store where private keys are stored
        store_name = "eth-accounts"

        # Path to the file containing the operator's Ethereum address
        address_path = "../node-data/test/keys/operator_1.acc"

    [key_management.encryption_key]
        manager_type = "Qkm"

        # URL to QKM service
        qkm_url = "http://qkm:8080"

        # Name of the store where encryption keys are stored
        store_name = "encryption-keys"

        # Path to the file containing the ID of the operator's encryption key
        key_id_path = "../node-data/test/keys/operator_1.acc"
    ```
