---
Description: ConsenSys Rollups configuration file options.
---

# Configuration file

Configuration uses [TOML files](https://toml.io/) to configure the manager and engine components of ConsenSys Rollups.
Each component requires its own configuration file.

The following section describes the configuration file options for the [manager](#manager) and [engine](#engine).

## Manager

{!global/Manager-Configuration-File.md!}

The operator's manager supports the following settings.

### `database`

The database section contains the details to connect to the manager's database.

#### `host`

Database host address.

#### `password`

User's password.

#### `port`

Database port number.

#### `username`

User name.

#### `schema`

Database schema namespace.

### `kafka`

This section contains the [Kafka](https://kafka.apache.org/) settings:

#### `batch_updates_topic`

Topic used by Kafka. The default is `sumo-state-updates`.

#### `brokers`

Address of the [Kafka brokers]. The default is `localhost:9092`.

#### `invalid_operations_topic`

Kafka topic used to publish invalid operations.

#### `invalid_operations_consumer_group_id`

Id of the Kafka consumer group for the invalid operations topic.

### `api`

The details to access the manager's APIs.

#### `http_port`

HTTP JSON-RPC listening port to access the manager's APIs.

#### `websocket_port`

WebSocket JSON-RPC listening port to access the manager's APIs.

### `engine`

Details of the operator's engine component.

#### `uri`

Address (host and port) of the engine.

### `ha`

Configure ConsenSys Rollups to be [highly available](../Concepts/High-Availability.md).

### `ha.kafka`

Configure the [highly available](../Concepts/High-Availability.md) Kafka settings.

#### `brokers`

Address of the highly available [Kafka brokers].

#### `pending_operations_replication_topic`

Topic name used for replication. This instance replicates the operation requests served by the JSON-RPC APIs.

#### `pending_operations_replication_partitions`

Number of partitions used to load balance messages. If the values do not match, then for safety
reasons, the Manager is shut down.

!!! important

    This number should never change once messages start to be written, otherwise the correct order won't be preserved.

#### `consumer_group_name`

Name of the Kafka consumer group for this operator instance. Used for offset tracking. It must:

* Never change during the operator lifetime
* Be unique for each operator instance

#### `pending_operations_replication_topics_to_copy`

List of topics where other instances publish pending operations. The operations are replicated to
the local database and engine.

## Engine

{!global/Engine-Configuration-File.md!}

The engine supports the following settings.

### `crypto_suite`

Rollup's cryptographic schema. Options are `Native` and `Bn`. The default is `Bn`.

### `outbound_transfer_batch_size`

Number of outbound transfers in a batch. The default is `0`.

### `rollup_type`

Type of rollup to implement. This option cannot be changed after the rollup's creation.
Use `PaZkp` for [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).
The default is `PaZkp`.

### `server_addr`

Operator's API address. The default is `0.0.0.0:5000`.

### `sig_check_thread_count`

Number of threads to use for verifying signatures. Must be less than the number of cores available.
The default is `2`.

### `smc_max_offset`

Maximum number of snapshots pending finalization, must be smaller or equal to the `max_offset`
value in the rollup smart contract. The default is `32`.

### `smc_transaction_batch_size`

Number of smart contract transactions in a batch. Includes inbound and outbound transfers.
The default is `3000`.

### `transaction_batch_size`

Number of transactions in a batch. The default is `2000`.

### `transaction_fetch_wait_time_in_ms`

Time to wait for new transactions or blocks. The default is `100`.

### `trust_origin`

Specify whether to trust the origin of the request. If `true`, then the operator skips signature
verification. The default is `false`.

### `blockchain_connector`

This section contains accepts the following settings to connect to the blockchain.

#### `confirmations`

Number of confirmations for an Ethereum transaction. For example, calls to `submitTransactions` or
`voteFor` in the rollup smart contract.

#### `connector_type`

Connector type used to connect to the blockchain client. The default is `Web3`.

#### `forced_transaction_timeout`

Forced transaction timeout (in Ethereum blocks). The rollup will freeze if there is an unprocessed
forced transaction exists that is older than the current block number minus the timeout value.
The default is `86400`.

#### `gas_limit_batch`

Gas limit for a call to `submitTransactions` in the rollup smart contract. The default is `100000000`.

#### `gas_limit_vote`

Gas limit for a call to 'voteFor' in the rollup smart contract. The default is `100000000`.

#### `lookahead`

Maximum number of blocks to fetch in a single query when the operator is catching up.

#### `nb_of_blocks_to_finalization`

Number of successors a block requires before being considered final.

#### `operator_address_path`

Path to the file containing the operator's Ethereum private and public keys.

#### `smc_abi_path`

Path to the file containing the rollup smart contract ABI.

#### `url`

URL of the blockchain client. The default is `http://localhost:8545`.

### `database`

The database section contains the details to connect to the engines's database.

#### `database`

Database name.

#### `database_type`

Database type. Valid options are `Postgres` and `Dummy`. The default is `Postgres`.

!!! warning

    The `Dummy` option is used for testing to start the operator without the database dependency.

    This option must not be used in production.

#### `host`

Database host address.

#### `password`

User's password.

#### `port`

Database port number.

#### `username`

User name.

### `kafka`

This section contains the [Kafka](https://kafka.apache.org/) settings:

#### `batch_updates_topic`

Topic used by Kafka. The default is `sumo-state-updates`.

#### `brokers`

Address of the [Kafka brokers]. The default is `localhost:9092`.

#### `kafka_type`

Kafka type options are `Real` and `Dummy`. The default is `Real`.

!!! warning

    The `Dummy` option is used for testing to start the operator without the Kafka dependency.

    This option must not be used in production.

#### `timeout_ms`

Timeout period for Kafka streams. The default is `5000`.

### `state_manager`

This section manages contains settings that manage the state details of the rollup.

#### `arity`

The arity of the rollup. The default is `2`.

#### `capacity`

Targeted capacity of the rollup to ensure memory is immediately allocated correctly. Can be changed
after the rollup's creation.

#### `depth`

The depth of the rollup's Merkle tree. The rollup's maximum capacity is $2^\text{depth}$.
The parameter cannot be changed after the rollup's creation.

#### `thread_count`

Number of threads to use when recalculating the hash tree during startup. Set this to the number of
cores available.

### `transaction_manager`

This section contains parameters to manage the transaction queue.

#### `completeds_length`

Number of historical batches kept in memory. The default is `100`.

#### `dynamic_batch_interval_ms`

During low transaction periods it can take a long time before the operator collects enough
standard transactions to create a batch. This setting allows you to create partially filled batches.

The operator attempts to submit the partially filled batch after the specified interval of
the previous submission. This setting is optional, and if not set, the dynamic batch creation
functionality is disabled.

#### `incoming_capacity`

Number of transactions that can be processed by the insert poller at once. Setting this parameter to
`1000` means the limit is 1000 transactions. Exceeding this capacity means the transactions are
rejected until the poller empties the buffer. The default is `100`.

#### `insert_poller_epoch_ms`

Interval at which transactions are moved from incoming to pending state. The default is `100`.

#### `pending_capacity`

Number of pending transactions that can be stored. Different types of transactions are stored
separately. Ensure you set this value high enough since exceeding this capacity involves a full copy
of the structure with every write. The default is `100`.

#### `processing_capacity`

Number of batches that can be simultaneously stored in processing. Ensure you set this value
high enough since exceeding this capacity involves a full copy of the structure with every write.
The default is `100`

<!-- links -->
[Kafka brokers]: https://jaceklaskowski.gitbooks.io/apache-kafka/content/kafka-properties-bootstrap-servers.html
