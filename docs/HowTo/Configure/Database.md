---
Description: How to configure the database
---

# Database

The operator supports [PostgreSQL](https://www.postgresql.org/) as the database management system.

Configure access to the database in the [configuration file](Config-Files.md).

!!! example "Kafka configuration example"

    ```toml
    [database]
        database_type = "Postgres"
        username = "postgres"
        password = "postgres"
        host = "localhost"
        port = "5432"
        database = "engine_operator_1"
    ```
