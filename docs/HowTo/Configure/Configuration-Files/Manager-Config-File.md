---
Description: The manager's configuration file.
---

# Configuration files

Supply a valid TOML configuration file when starting the operator's manager component. The manager
handles user requests and communicates with the engine.

The file contains the settings to connect to the engine and specify the API details.

!!!example "Sample TOML configuration file"

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

The operator's manager supports the following settings.

## `database`

The database section contains the details to connect to the manager's database.

### `host`

Database host address.

### `password`

User's password.

### `port`

Database port number.

### `username`

User name.

### `schema`

Database schema namespace.

## `kafka`

This section contains the [Kafka](https://kafka.apache.org/) settings:

### `batch_updates_topic`

Topic used by Kafka. Defaults to `sumo-state-updates`.

### `brokers`

Address of the [Kafka brokers]. Defaults to `localhost:9092`.

## `api`

The details to access the manager's APIs.

### `http_port`

The  HTTP JSON-RPC listening port to access the manager's APIs.

### `websocket_port`

The Websockets JSON-RPC listening port to access the manager's APIs.

## `engine`

Details of the operator's engine component.

### `uri`

The address (host and port) of the engine.

<!-- links -->
[Kafka brokers]: https://jaceklaskowski.gitbooks.io/apache-kafka/content/kafka-properties-bootstrap-servers.html
