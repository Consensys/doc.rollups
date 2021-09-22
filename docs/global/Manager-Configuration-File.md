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
        batch_updates_topic = "sumo-state-updates"

    [api]
        http_port = 6000
        websocket_port = 6001

    [engine]
        uri = "http://engine:5000"
    ```
