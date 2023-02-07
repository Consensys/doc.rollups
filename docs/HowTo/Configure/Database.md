---
title: Database
description: How to configure the database
sidebar_position: 2
---

# Database

ConsenSys Rollups support [PostgreSQL](https://www.postgresql.org/) as the database management system.

Configure access to the database for the [engine](Configuration-File.md#engine-configuration-file) and
[manager](Configuration-File.md#manager-configuration-file) components separately.


```toml title="Database configuration example"
[database]
    database_type = "Postgres"
    username = "postgres"
    password = "postgres"
    host = "localhost"
    port = "5432"
    database = "engine_operator_1"
```
