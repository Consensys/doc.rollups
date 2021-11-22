!!!example "Sample Manager configuration file"

    ```toml
    [database]
        username = "postgres"
        password = "postgres"
        host = "postgres"
        port = "5432"
        schema = "manager_operator_1"

    [kafka]
        brokers = "kafka-broker:9092"
        batch_updates_topic = "operator1_batch_updates"
        invalid_operations_topic = "operator1_invalid_operations"
        invalid_operations_consumer_group_id = "operator1-invalid-operations-manager-consumer"

    [api]
        http_port = 6000
        websocket_port = 6001

    [engine]
        uri = "http://engine:5000"

    [ha]

    [ha.kafka]
    brokers = "kafka-broker:29092"
    pending_operations_replication_topic = "operator1_i0_pending_operations"
    pending_operations_replication_partitions = 3
    consumer_group_name = "operator1_i0_pending_op_consumer"
    pending_operations_replication_topics_to_copy = ["operator1_i1_pending_operations"]
    ```
