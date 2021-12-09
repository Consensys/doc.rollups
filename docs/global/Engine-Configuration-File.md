!!!example "Sample Engine configuration file"

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
        kafka_type = "Dummy"
        brokers = "localhost:9092"
        timeout_ms = 5000
        batch_updates_topic = "sumo-state-updates"
    ```
