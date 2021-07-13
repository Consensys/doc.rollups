---
Description: The Sumo rollup configuration file.
---

# Configuration files

Supply a valid TOML configuration file when starting the operator. The file contains the settings
to manage the rollup, and connect to the blockchain client and database.

!!!example "Sample TOML configuration file"

    ```toml
    transaction_batch_size = 2
    smc_transaction_batch_size = 2
    outbound_transfer_batch_size = 2
    server_addr = "0.0.0.0:5000"
    rollup_type = "Consensus"
    crypto_suite = "Native"
    trust_origin = true
    transaction_fetch_wait_time_in_ms = 100
    smc_max_offset = 32
    sig_check_thread_count = 2
    
    [state_manager]
        arity = 2
        depth = 28
        capacity = 10_000_000
        thread_count = 4
    
    [transaction_manager]
        incoming_capacity = 1_000_000
        pending_capacity = 1_000_000
        processing_capacity = 100_000
        completeds_length = 20
        insert_poller_epoch_ms = 50
    
    [database]
        database_type = "Postgres"
        username = "postgres"
        password = "postgres"
        host = "localhost"
        port = "5432"
        database = "engine_operator_1"
    
    [blockchain_connector]
        connector_type = "Web3"
        url = "http://localhost:8545"
        operator_address_path = "node-data/test/keys/eth_account_1.acc"
        smc_abi_path = "smart-contract/data/rollup.json"
        lookahead = 50
        nb_of_blocks_to_finalization = 3
        gas_limit_batch = 100_000_000
        gas_limit_vote = 100_000_000
        confirmations = 3
        forced_transaction_timeout = 86400
    
    [kafka]
        kafka_type = "Dummy"
        brokers = "localhost:9092"
        timeout_ms = 5000
        batch_updates_topic = "sumo-state-updates"
    ```

The operator supports the following settings.

## `crypto_suite`

The rollup's cryptographic schema. Options are `Native` and `Bn`. Defaults to `Bn`.

## `outbound_transfer_batch_size`

Number of outbound transfers in a batch. Defaults to `0`.

## `rollup_type`

Type of rollup to implement. This option cannot be changed after the rollup's creation. Defaults to
`PaZkp`.

## `server_addr`

Operator's API address. Defaults to `0.0.0.0:5000`.

## `sig_check_thread_count`

Number of threads to use for verifying signatures. Must be less than the number of cores available.
Defaults to `2`.

## `smc_max_offset`

Maximum number of snapshots pending finalization, must be smaller or equal to the `max_offset`
value in the rollup smart contract. Defaults to `32`.

## `smc_transaction_batch_size`

Number of smart contract transactions in a batch. Includes inbound and outbound transfers.
Defaults to `3000`.

## `transaction_batch_size`

Number of transactions in a batch. Defaults to `2000`.

## `transaction_fetch_wait_time_in_ms`

Time to wait for new transactions or blocks. Defaults to `100`.

## `trust_origin`

Specify whether to trust the origin of the request. If `true`, then the operator skips signature
verification. Defaults to `false`.

## `blockchain_connector`

This section contains accepts the following settings to connect to the blockchain.

### `confirmations`

Number of confirmations for an Ethereum transaction. For example, calls to `submitTransactions` or
`voteFor` in the rollup smart contract.

### `connector_type`

Connector type used to connect to the blockchain client. Defaults to `Web3`.

### `forced_transaction_timeout`

Forced transaction timeout (in Ethereum blocks). The rollup will freeze if there is an unprocessed
forced transaction exists that is older than the current block number minus the timeout value.
Defaults to `86400`.

### `gas_limit_batch`

Gas limit for a call to `submitTransactions` in the rollup smart contract. Defaults to `100000000`.

### `gas_limit_vote`

Gas limit for a call to 'voteFor' in the rollup smart contract. Defaults to `100000000`.

### `lookahead`

Maximum number of blocks to fetch in a single query when the operator is catching up.

### `nb_of_blocks_to_finalization`

Number of successors a block requires before being considered final.

### `operator_address_path`

Path to the file containing the operator's Ethereum private and public keys.

### `smc_abi_path`

Path to the file containing the rollup smart contract ABI.

### `url`

URL of the blockchain client. Defaults to `http://localhost:8545`.

## `database`

The database section contains the details to connect to the opertor's database.

### `database`

Database name.

### `database_type`

Database type. Valid options are `Postgres` and `Dummy`. Defaults to `Postgres`.

!!! warning

    The `Dummy` option is used for testing to start the operator without the database dependency.
    This option must not be used in production.

### `host`

Database host address.

### `password`

User's password.

### `port`

Database port number.

### `username`

User name.

## `kafka`

This section contains the [Kafka](https://kafka.apache.org/) settings:

### `batch_updates_topic`

Topic used by Kafka. Defaults to `sumo-state-updates`.

### `brokers`

Address of the Kafka brokers. Defaults to `localhost:9092`.

### `kafka_type`

Kafka type options are `Real` and `Dummy`. Defaults to `Real`.

!!! warning

    The `Dummy` option is used for testing to start the operator without the Kafka dependency.
    This option must not be used in production.

### `timeout_ms`

Timeout period for Kafka streams. Defaults to `5000`.

## `state_manager`

This section manages contains settings that manage the state details of the rollup.

### `arity`

The arity of the rollup. Defaults to `2`.

### `capacity`

Targeted capacity of the rollup to ensure  memory is immediately allocated correctly. Can be changed
after the rollup's creation.

### `depth`

The depth of the rollup's merkle tree. The rollup's maximum capacity is 2<sup>depth</sup>.
The parameter cannot be changed after the rollup's creation.

### `thread_count`

Number of threads to use when recalculating the hash tree during startup. Set this to the number of
cores available.

## `transaction_manager`

This section contains parameters to manage the transaction queue.

### `completeds_length`

Number of historical batches kept in memory. Defaults to `100`.

### `dynamic_batch_interval_ms`

During low transaction periods it can take a long time before the operator collects enough
standard transactions to create a batch. This setting allows you to create partially filled batches.

The operator attempts to submit the partially filled batch after the specified interval of
the previous submission. This setting is optional, and if not set, the dynamic batch creation
functionality is disabled.

### `incoming_capacity`

Number of transactions that can be processed by the insert poller at once. Setting this parameter to
`1000` means the limit is 1000 transactions. Exceeding this capacity means the transactions are
rejected until the poller empties the buffer. Defaults to `100`.

### `insert_poller_epoch_ms`

Interval at which transactions are moved from incoming to pending state. Defaults to `100`.

### `pending_capacity`

Number of pending transactions that can be stored. Different types of transactions are stored
separately. Ensure you set this value high enough since exceeding this capacity involves a full copy
of the structure with every write. Defaults to `100`.

### `processing_capacity`

Number of batches that can be simultaneously stored in processing. Ensure you set this value
high enough since exceeding this capacity involves a full copy of the structure with every write.
Defaults to `100`
