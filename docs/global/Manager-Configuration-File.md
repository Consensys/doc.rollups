!!!example "Sample Manager configuration file"

    ```toml
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
        http2_pool_size = 100
        connections_pool_size = 10

    [engine]
        uri = "http://engine-i1:5000"
        type = "Real"

    [ha]

    [ha.kafka]
        brokers = "kafka-broker:29092"
        pending_operations_replication_topic = "operator1_i1_pending_operations"
        pending_operations_replication_partitions = 3
        consumer_group_name = "operator1_i1_replicatior"
        pending_operations_replication_topics_to_copy = ["operator1_i0_pending_operations"]
    ```
