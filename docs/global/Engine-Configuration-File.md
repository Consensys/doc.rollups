!!!example "Sample Engine configuration file"

    ```toml
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
