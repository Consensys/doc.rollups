---
title: Configuration file
description: Manager and engine configuration files
sidebar_position: 1
---

# Use configuration files

Configuration files are used to configure the manager and engine components of ConsenSys Rollups. The files are
TOML formatted, and each component requires its own configuration file.

## Manager configuration file

The manager receives transactions and communicates with the engine.
The [configuration file options](../../Reference/Configuration-File.md#manager-configuration) allow you to specify the
address of the engine and manager APIs.

```toml title="Sample Manager configuration file"
[database]
    username = "postgres"
    password = "postgres"
    host = "postgres"
    port = "5432"
    schema = "operator1_i1_manager"
    pool_size = 10

[kafka]
    brokers = "kafka-broker:29092"
    batch_updates_topic = "operator1_i1_batch_updates"
    invalid_operations_topic = "operator1_i1_invalid_operations"
    invalid_operations_consumer_group_id = "operator1-i1-invalid-operations-manager-consumer"

[vertx_config]
    max_event_loop_execution_time_millis = 200
    max_worker_thread_execution_time_millis = 200
    log_stacktrace_threshold_millis = 500
    http_verticles = 2
    websocket_verticles = 2

[api]
    http_port = 6010
    websocket_port = 6011
    monitoring_port = 6012
    version = "0"

[api.auth]
    rollup_account_max_token_duration = "PT3H"
    api_account_token_duration = "PT4H"
    clock_leniency = "PT20S"

[http_client]
    connections_pool_size = 10

[engine]
    uri = "http://engine-i1:5000"

[ha]

[ha.kafka]
    brokers = "kafka-broker:29092"
    pending_operations_replication_topic = "operator1_i1_pending_operations"
    pending_operations_replication_partitions = 3
    consumer_group_name = "operator1_i1_replicatior"
    pending_operations_replication_topics_to_copy = ["operator1_i0_pending_operations"]
```

### Override manager configuration options

You can override any manager configuration file options by:

- Specifying multiple configuration files on the command line.
  If the same configuration options are specified across multiple files, the values defined in the latest file are used,
  overriding the former files' values.
  For example, if you specify `config.toml overrides1.toml overrides2.toml` on the command line,
  the values in `overrides2.toml` override the values in `overrides1.toml`, which override the values in `config.toml`.

- Using the `-Dconfig.override` command line option prefix.
  For example, to override the [Kafka brokers](../../Reference/Configuration-File.md#brokers) option with the value
  `localhost:9092`, specify `-Dconfig.override.kafka.brokers=localhost:9092` on the command line.
  This option overrides values specified in configuration files.

## Engine configuration file

The engine builds a batch of transactions and sends it to the blockchain.
The [configuration file options](../../Reference/Configuration-File.md#engine-configuration) allow you to configure the
rollup details, and access the blockchain.

```toml title="Sample Engine configuration file"
transaction_batch_sizes = [1]
forced_transaction_batch_sizes = [1]
outbound_transfer_batch_sizes = [1]

listen_address= "0.0.0.0:5000"
rollup_type = "PaZkp"
crypto_suite = "Bn"
smart_contract_max_offset = 32
trust_origin = false

[state_manager]
account_merkle_tree_depth = 28
account_merkle_tree_capacity = 10_000_000
balance_merkle_tree_depth = 8
money_order_merkle_tree_depth = 32
money_order_initial_capacity = 4
money_order_batch_merkle_tree_depth = 12

[transaction_manager]
incoming_capacity = 100000
pending_capacity = 1000000
processing_capacity = 100000
completed_length = 200
insert_poller_epoch_ms = 50

[database]
database_type = "Postgres"
username = "postgres"
password = "postgres"
host = "postgres"
port = "5432"
database = "operator1_engine"

[blockchain_connector]
connector_type = "Web3"
url = "http://blockchain-node:8545"
smart_contract_abi_path = "/smart_contract/data/rollup.json"
lookahead_blocks = 50
blocks_to_finalization = 1
gas_limit_batch = 100000000
gas_limit_vote = 100000000
confirmations = 1
forced_transaction_timeout_blocks = 86400

[key_management.account_key]
manager_type = "Qkm"
qkm_url = "http://qkm:8080"
store_name = "eth-accounts"
address_path = "../node-data/test/keys/operator_1.acc"

[key_management.encryption_key]
manager_type = "Qkm"
qkm_url = "http://qkm:8080"
store_name = "encryption-keys"
key_id_path = "../node-data/test/keys/operator_1.acc"

[kafka]
kafka_type = "Real"
brokers = "kafka-broker:29092"
timeout_ms = 60000
batch_updates_topic = "operator1_batch_updates"
invalid_operations_topic = "operator1_invalid_operations"

[prover]
prover_type = "Dummy"
job_address = "https://prover:9002"
```

### Override engine configuration options

You can override any engine configuration file options by specifying an additional configuration file using the
`--override` command line option one or more times.
If the same configuration options are specified across multiple files, the values defined in the latest file are used,
overriding the former files' values.
For example, if you specify `--config config.toml --override overrides1.toml --override overrides2.toml`, the values in
`overrides2.toml` override the values in `overrides1.toml`, which override the values in `config.toml`.
