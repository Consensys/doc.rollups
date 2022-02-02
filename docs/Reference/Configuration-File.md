---
Description: ConsenSys Rollups configuration file options.
---

# Configuration file

Configuration uses [TOML files](https://toml.io/) to configure the manager and engine components of ConsenSys Rollups.
Each component requires its own configuration file.

The following section describes the configuration file options for the [manager](#manager-configuration) and
[engine](#engine-configuration).

## Manager configuration

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

ID of the Kafka consumer group for the invalid operations topic.

!!! important

    We recommend you use a different ID for each operator instance.

### `vertx_config`

This section contains the [Vert.x](https://vertx.io/docs/) settings.

#### `max_event_loop_execution_time_millis`

Value of max [event loop execute time](https://vertx.io/docs/apidocs/io/vertx/core/VertxOptions.html#setMaxEventLoopExecuteTime-long-), in milliseconds.

#### `max_worker_thread_execution_time_millis`

Value of [max worker execute time](https://vertx.io/docs/apidocs/io/vertx/core/VertxOptions.html#setMaxWorkerExecuteTime-long-), in milliseconds.

#### `log_stacktrace_threshold_millis`

Threshold time in milliseconds to trigger a warning containing a [stack trace](https://vertx.io/docs/apidocs/io/vertx/core/VertxOptions.html#setWarningExceptionTime-long-).

#### `http_verticles`

Number of processing units for HTTP requests.

!!! important

    Don't set `http_verticles` higher than the maximum number of CPUs in the underlying machine.

#### `websocket_verticles`

Number of processing units for WebSocket requests.

!!! important

    Don't set `websocket_verticles` higher than the maximum number of CPUs in the underlying machine.

### `api`

The details to access the manager's APIs.

#### `http_port`

HTTP JSON-RPC listening port to access the manager's APIs.

#### `websocket_port`

WebSocket JSON-RPC listening port to access the manager's APIs.

#### `monitoring_port`

HTTP port for monitoring the following endpoints: `/health`, `/metrics`, and `/live`.

#### `version`

JSON-RPC API version.

### `api.auth`

Details on API authorizations.

#### `rollup_account_max_token_duration`

Maximum time a Rollups account session can last. Tokens are issued on login and can't have a duration time longer than this.

#### `api_account_token_duration`

Duration of API accounts tokens.

!!! note

    API accounts are special accounts that can read the state of all Rollups accounts. Credentials and permissions for these accounts are specified in a `credentials.toml` file.

#### `clock_leniency`

Amount of time desynchronization tolerated between the users and server for JWT token validation.

### `http_client`

Details of the HTTP client component.

#### `http2_pool_size`

Pool size of HTTPV2 connections from the manager and engine components.

#### `connections_pool_size`

Pool size of HTTPV1 connections from the manager and engine components.

### `engine`

Details of the operator's engine component.

#### `uri`

Address (host and port) of the engine.

#### `type`

Type options are `Real` and `Fake`. The default is `Real`.

!!! warning

    The `Fake` option is used for testing to start the operator without the engine dependency.

    This option must not be used in production. This option will be deprecated in a future release.

### `ha`

Configure ConsenSys Rollups to be [highly available](../Concepts/High-Availability.md).

### `ha.kafka`

Configure the [highly available](../Concepts/High-Availability.md) Kafka settings.

#### `brokers`

Address of the highly available [Kafka brokers].

#### `pending_operations_replication_topic`

Topic name used for replication. This instance replicates the operation requests served by the JSON-RPC APIs.

!!! important

    Each operator instance must write to a different topic, For example:

    * Instance 0 writes to `operator1_i0_pending_operations`.
    * Instance 1 writes to `operator1_i1_pending_operations`.

#### `pending_operations_replication_partitions`

Number of partitions used to load balance messages. If the values do not match, then for safety
reasons the Manager shuts down.

!!! important

    This number should never change once messages start to be written, otherwise the correct order won't be preserved.

#### `consumer_group_name`

Name of the Kafka consumer group for this operator instance. Used for offset tracking. It must:

* Never change during the operator lifetime.
* Be unique for each operator instance.

#### `pending_operations_replication_topics_to_copy`

List of topics where other instances publish pending operations. The operations are replicated to
the local database and engine.

## Engine configuration

{!global/Engine-Configuration-File.md!}

The engine supports the following settings.

### `crypto_suite`

Rollup's cryptographic schema. Options are `Native` and `Bn`. The default is `Bn`.

### `forced_transaction_batch_size`

Number of forced transactions in a batch.
The default is `3000`.

### `listen_address`

Operator's API address. The default is `0.0.0.0:5000`.

### `outbound_transfer_batch_size`

Number of outbound transfers in a batch. The default is `0`.

### `rollup_type`

Type of rollup to implement. This option cannot be changed after the rollup's creation.
Use `PaZkp` for [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).
The default is `PaZkp`.

### `smart_contract_max_offset`

Maximum number of snapshots pending finalization, must be smaller or equal to the `max_offset`
value in the rollup smart contract. The default is `32`.

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

#### `forced_transaction_timeout_blocks`

Forced transaction timeout (in Ethereum blocks). The rollup will freeze if there is an unprocessed
forced transaction exists that is older than the current block number minus the timeout value.
The default is `86400`.

#### `gas_limit_batch`

Gas limit for a call to `submitTransactions` in the rollup smart contract. The default is `100000000`.

#### `gas_limit_vote`

Gas limit for a call to 'voteFor' in the rollup smart contract. The default is `100000000`.

#### `lookahead_blocks`

Maximum number of blocks to fetch in a single query when the operator is catching up.

#### `blocks_to_finalization`

Number of successors a block requires before being considered final.
Set to `1` for a private deployment, or `3` or more for Mainnet.

#### `smart_contract_abi_path`

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

Topic used by Kafka. The default is `operator1_state_updates`.

#### `brokers`

Address of the [Kafka brokers]. The default is `localhost:9092`.

#### `invalid_operations_topic`

Topic where information about invalid operations are sent. The default is `operator1_invalid_operations`.

#### `kafka_type`

Kafka type options are `Real` and `Dummy`. The default is `Real`.

!!! warning

    The `Dummy` option is used for testing to start the operator without the Kafka dependency.

    This option must not be used in production.

#### `timeout_ms`

Timeout period for Kafka streams. The default is `5000`.

### `key_management.account_key`

This section contains [key management](../HowTo/Configure/KeyManagement.md) account key settings:

#### `address_path`

Path to the file containing the operator's Ethereum address.
This is required only when using a `Qkm` [manager type](#manager_type).

#### `manager_type`

Key manager type.
Possible values are:

* `Filesystem` - Private keys are kept in the filesystem.
* `Qkm` - Keys are managed by [Quorum Key Manager](https://docs.quorum-key-manager.consensys.net/en/stable/).

#### `operator_key_path`

Path to the file containing the operator's private key.
This is required only when using a `Filesystem` [manager type](#manager_type).

#### `qkm_url`

URL to the Quorum Key Manager service.
This is required only when using a `Qkm` [manager type](#manager_type).

#### `store_name`

Name of the [store](https://docs.quorum-key-manager.consensys.net/en/stable/Concepts/Stores/) where Ethereum private
keys are stored.
This is required only when using a `Qkm` [manager type](#manager_type).

### `key_management.encryption_key`

This section contains [key management](../HowTo/Configure/KeyManagement.md) encryption key settings.
Setting this section is optional.

#### `encryption_key_path`

Path to the file containing the operator's encryption key.
This is required only when using a `Filesystem` [manager type](#manager_type).

#### `key_id_path`

Path to the file containing the ID of the operator's encryption key.
This is required only when using a `Qkm` [manager type](#manager_type).

#### `manager_type`

Key manager type.
Possible values are:

* `Filesystem` - Private keys are kept in the filesystem.
* `Qkm` - Keys are managed by [Quorum Key Manager](https://docs.quorum-key-manager.consensys.net/en/stable/).

#### `qkm_url`

URL to the Quorum Key Manager service.
This is required only when using a `Qkm` [manager type](#manager_type).

#### `store_name`

Name of the [store](https://docs.quorum-key-manager.consensys.net/en/stable/Concepts/Stores/) where encryption keys are
stored.
This is required only when using a `Qkm` [manager type](#manager_type).

### `prover`

This section contains settings for the prover. The prover is only applicable for [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).

#### `prover-type`

Type of the prover. Possible values are `Dummy` or `Real`.

#### `job_address`

Address for gRPC interface for managing prover jobs. The default is `https://127.0.0.1:9002`.

### `state_manager`

This section manages contains settings that manage the state details of the rollup.

#### `account_merkle_tree_capacity`

Targeted capacity of the rollup to ensure memory is immediately allocated correctly. Can be changed
after the rollup's creation.

#### `account_merkle_tree_depth`

The depth of the rollup's account tree. The rollup's maximum capacity is $2^\text{depth}$.
The parameter cannot be changed after the rollup's creation.

#### `arity`

The arity of the rollup. The default is `2`.

#### `balance_merkle_tree_depth`

Depth of the account's balance tree.
The rollup's maximum capacity (in number of tokens) is $2^\text{balance_merkle_tree_depth}$.

#### `money_order_batch_merkle_tree_depth`

Depth of the money order batch.
The maximum number of money orders that can be created/redeemed in a single batch is $2^\text{money_order_batch_merkle_tree_depth}$.
Applicable only for [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).

#### `money_order_initial_capacity`

Capacity pre-allocated for money order history call tree.
Applicable only for [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).

#### `money_order_merkle_tree_depth`

Depth of money orders history call tree, must be greater than 1 and less than or equal to 32.
The maximum number of money order batches that can be created is $2^\text{money_order_merkle_tree_depth}$.
Applicable only for [partially anonymous rollups](../Concepts/Rollups/Partially-Anonymous-Rollups.md).

#### `thread_pool_thread_count`

Number of threads to use for signature validation and root hash calculation.
Set this to less than the number of cores available.

### `transaction_manager`

This section contains parameters to manage the transaction queue.

#### `completed_length`

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
The default is `100`.

<!-- links -->
[Kafka brokers]: https://jaceklaskowski.gitbooks.io/apache-kafka/content/kafka-properties-bootstrap-servers.html
