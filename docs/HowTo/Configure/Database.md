---
Description: How to configure the database
---

# Database

The operator supports [PostgreSQL](https://www.postgresql.org/) as the database management system.

Configure access to the database for the [engine](Configuration-Files/Engine-Config-File.md) and
[manager](Configuration-Files/Manager-Config-File.md) components.

!!! example "Database configuration example"

    ```toml
    [database]
        database_type = "Postgres"
        username = "postgres"
        password = "postgres"
        host = "localhost"
        port = "5432"
        database = "engine_operator_1"
    ```
